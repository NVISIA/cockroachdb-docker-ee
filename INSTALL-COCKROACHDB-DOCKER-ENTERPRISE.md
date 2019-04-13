# Set up Cockroach DB on Docker EE Bare Metal Cluster

## (Manual Persistent Volume Provisioning)

```bash
# Set up RBAC for Cockroach DB
$ kubectl apply -f cdb-sa-roles-cdb-ns.yaml
$ kubectl apply -f cdb-sa-roles-cdb-global.yaml


#set prefered namespace
$ kubectl config set-context $(kubectl config current-context) --namespace=cockroachdb

# Baremetal need Persisten Volumes pre-configured
#...could have used NFS, but speed concerns and don't really need it
$ kubectl apply -f set-up-cdb-pv.yaml

# Deploy Cockroach Pods
$ kubectl apply -f deploy-cdb.yaml

# Pods - init phase create certificates for secure db cluster node communication 
# Generate certificate singing requestm then 

$ kubectl get csr
$ kubectl describe csr cockroachdb.node.cockroachdb-0
$ kubectl certificate approve cockroachdb.node.cockroachdb-0
$ kubectl certificate approve cockroachdb.node.cockroachdb-1
$ kubectl certificate approve cockroachdb.node.cockroachdb-2

# Review kube resources
$ kubectl get pods
$ kubectl get persistentvolumes

# Intialize the cluster with
#   Note: This is not needed if you mounted the file system from a previous cluster - CDB  will resurect the old cluster. 
$ kubectl apply -f cdb-cluster-init.yaml

$ kubectl certificate approve cockroachdb.client.root 

$ kubectl get job cluster-init-secure

$ kubectl apply -f cdb-launch-client.yaml
```

## Test out the new cluster
```bash
$ kubectl exec -it cockroachdb-client-secure -- ./cockroach sql --certs-dir=/cockroach-certs --host=cockroachdb-public

root@cockroachdb-public:26257/defaultdb>
```

```sql
> CREATE DATABASE bank;
> CREATE TABLE bank.accounts (id INT PRIMARY KEY, balance DECIMAL);
> INSERT INTO bank.accounts VALUES (1, 1000.50);
> SELECT * FROM bank.accounts;
> CREATE USER roach WITH PASSWORD 'Q7gc8rEdS';
> SET CLUSTER SETTING timeseries.resolution_10s.storage_duration = '24h0m0s';
> SET CLUSTER SETTING server.time_until_store_dead = '10m0s'; 
>\q 
```

## Set up tunned to NodePort Service on ephemeral port

```bash
kubectl get svc cockroachdb-public
NAME                 TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                          AGE
cockroachdb-public   NodePort   10.96.195.128   <none>        26257:33614/TCP,8080:33510/TCP   53m
```
Using putty or SSH, set up tunnel to any (worker/non-master) node in your cluster as listed above (in my case port 33501 - your port will be probably be different).

The tunnel will look somethong like:
| Local Port | Remote Server/Port       |
| ---------- | ------------------------ |
| 8080       | Remote: 10.10.1.39:33501 |

## Then use your browser to tunnel to the Cockroach DB UI: https://127.0.0.1:8080

### Accept self-singed cert warnings

### Use user: roach and password: Q7gc8rEdS

## Runs load tests...
[Here are the instructions...](DEMO-WORKLOAD-APP.md)

## Clean up

### clean up kube resources

```bash
$ kubectl delete -f cdb-launch-client.yaml
$ kubectl delete -f deploy-cdb.yaml
$ kubectl delete pvc datadir-cockroachdb-0
$ kubectl delete pvc datadir-cockroachdb-1
$ kubectl delete pvc datadir-cockroachdb-2
$ kubectl delete -f set-up-cdb-pv.yaml
$ kubectl delete csr --all
$ kubectl delete -f cdb-sa-roles-cdb-global.yaml
$ kubectl delete -f cdb-sa-roles-cdb-ns.yaml
$ kubectl config set-context $(kubectl config current-context) --namespace=default
```
### Remove the cluster filesystem

On each Kube (worker) node...

```bash
$ sudo rm -rf /var/local/cdb-vol1
$ sudo rm -rf /var/local/cdb-vol2
$ sudo rm -rf /var/local/cdb-vol3
```
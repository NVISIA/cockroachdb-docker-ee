# Run a secure load generator application against Cockroach DB

Before you start make sure your cluster is set up...

See [install on local kube cluster](INSTALL-COCKROACHDB-LOCAL-KUBE.md)

See [install on remote Docker EE kube cluster](INSTALL-COCKROACHDB-DOCKER-ENTERPRISE.md)


## Install local Cockroach DB and test it


```bash
# fire up a cockroach DB secure client sql session...
$ kubectl exec -it cockroachdb-client-secure -- ./cockroach sql --certs-dir=/cockroach-certs --host=cockroachdb-public
```

```sql
-- verify you can connect to test DB
> SELECT * FROM bank.accounts;
```

## Run the a secure TPCC workload

### Use your local terminal or Docker EE CLI to deploy the workload job

```bash
$ kubectl apply -f test-app-secure-tpcc.yaml
# Check load generator job status 

# Check the job logs
$ kubectl logs job/tpcc-loadgen-secure-cdb

# Clean up! 
$ kubectl delete -f test-app-secure-tpcc.yaml
# or...
$ kubectl delete job tpcc-loadgen-secure-cdb
```

### Use your local terminal or Docker EE CLI to deploy the workload job

```bash
$ kubectl apply -f test-app-secure-kv.yaml
# Check load generator job status 

# Check the job logs
$ kubectl logs job/kv-loadgen-secure-cdb

# Clean up! 
$ kubectl delete -f test-app-secure-kv.yaml
# or...
$ kubectl delete job kv-loadgen-secure-cdb
```
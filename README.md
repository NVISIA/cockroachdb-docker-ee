# Distributed SQL for Microservices with Kubernetes
## Demo repo for Chicago Microservices Meetup with CockroachDB

## Demo topics 1 - Install without an persistent volume autoprovisioner

### Attempt to install using a Helm Chart

[Cockroach DB Docs - Helm install](https://www.cockroachlabs.com/docs/stable/orchestrate-cockroachdb-with-kubernetes.html#step-2-start-cockroachdb)

[Attempt details here...](ATTEMPT-HELM-INSTALL.md)

### Manual Kubernetes Install (assumes PV auto provisioner)
[Here are the Cockroach DB Docs as a starting point - Kubernetes manual install](https://www.cockroachlabs.com/docs/stable/orchestrate-cockroachdb-with-kubernetes.html#manual)

#### Running on Bare Metal Docker Enterprise OR Kubernetes on Docker Desktop using manual persistent volume provisioning

[Use Kubernetes on Docker Desktop or Docker EE to install a HA Cockroach DB Cluster](INSTALL-COCKROACHDB-DOCKER-ENTERPRISE.md)

## Demo topic 2 - Generate workloads

[Here are the workload generation instructions for TPCC and KV](DEMO-WORKLOAD-APP.md)

## Demo topic 3 - Simulate node failure

1. Apply workload 
2. Stop Docker on Kubernetes (worker) node
3. Watch CockroachDB UI

## Demo topic 4 - Clean up

### Cleanup 
Use the last section section of the [install document](INSTALL-COCKROACHDB-DOCKER-ENTERPRISE.md)

## Bonus topic - Sysdig monitoring of Docker EE

# Distributed SQL for Microservices with Kubernetes
## Demo repo for Chicago Microservices Meetup with CockroachDB

## Demo topics 1 - Installing without an persistent volume autoprovisioner

### Attempt to install using a Helm Chart

[Cockroach DB Docs - Helm install](https://www.cockroachlabs.com/docs/stable/orchestrate-cockroachdb-with-kubernetes.html#step-2-start-cockroachdb)

[Attempt details here...](ATTEMPT-HELM-INSTALL.md)

#### Running on Bare Metal Docker Enterprise OR Kubernetes on Docker Desktop requires manual persistent volume provisioning

[Use Kubernetes on Docker Desktop or Docker EE to install a HA Cockroach DB Cluster](INSTALL-COCKROACHDB-DOCKER-ENTERPRISE.md)

#### Footnote: Distributed application on distributes clusters

[Here are the Cockroach DB Docs as a starting point - Kubernetes manual install](https://www.cockroachlabs.com/docs/stable/orchestrate-cockroachdb-with-kubernetes.html#manual)
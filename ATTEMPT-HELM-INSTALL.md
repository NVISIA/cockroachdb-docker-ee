# Attemt to us Help Install

## Note: This will fail to install unless your Docker Enterprise environment is setup to with a Kube PV dynamic provisioner

[Kube docs are here](https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/#enabling-dynamic-provisioning)

### 1- Get and source client bundle for Docker Enterprise cluster as admin

### 2- From the admin client terminal...

```bash
admin:docker-ee$ helm init --service-account tiller

admin:docker-ee$ helm install --name my-release --set Secure.Enabled=true --set Storage=10Gi stable/cockroachdb
  ...fails without PV dynamic provisioning.
```

### Can't schedule cochroackdb to node with available PV based on PVC

So, it is onto the manual install: [Install with Docker Enterprise - Manual PV provisioning
](INSTALL-COCKROACHDB-DOCKER-ENTERPRISE.md)
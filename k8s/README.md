# K8S

## Cluster

[MicroK8s](https://microk8s.io) is used for a lightweight kubernetes cluster.
This is deployed on a Ubuntu Server VM in Proxmox VE.

## Continuous deployment - GitOps

The kubernetes cluster is set up with [FluxCD](https://fluxcd.io) for continuous deployment.

Flux uses GitOps practices and a pull-based deployment model.

The deployments are structured in a monorepo approach, see [Ways of structuring your repositories](https://fluxcd.io/flux/guides/repository-structure/).
Deployment manifests for all apps and services in the cluster are defined here in this repository for Flux to sync and deploy.

To **update flux**, run the following command and push the changes 
```
flux install 
--components-extra image-reflector-controller,image-automation-controller \
--export > ./k8s/clusters/production/flux-system/gotk-components.yaml
```

## Infrastructure

### Cloudflare Tunnel
A [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) is used to expose services online.
The tunnel client runs inside the cluster and routes traffic from cloudflare to the correct kubernetes service.

### Sealed secrets

[Sealed secrets](https://github.com/bitnami-labs/sealed-secrets?tab=readme-ov-file) controller is used to safely manage secrets in a GitOps way.
The controller runs in the cluster and decrypts the sealed secrets into regular kubernetes secrets.
The [public key](public-keys/pub-sealed-secrets.pem) can be used to encrypt the secret so that it can safely be stored in git.
The procedure is explained on the FluxCD website [fluxcd.io/flux/guides/sealed-secrets/](https://fluxcd.io/flux/guides/sealed-secrets/)
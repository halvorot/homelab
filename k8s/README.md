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
flux install \                                                                                                                  ⎈ microk8s
--components-extra image-reflector-controller,image-automation-controller \
--export > ./k8s/clusters/production/flux-system/gotk-components.yaml
```

## Infrastructure

### MetalLB

[MetalLB](https://metallb.io) is used as the load balancer to assign IP-addresses. 
Specifically to the ingress controller.

### Ingress controller

[ingress-nginx](https://github.com/kubernetes/ingress-nginx) is used as the Ingress controller for Kubernetes.
This acts as a reverse proxy and load balancer.

### Cert manager

[cert-manager](https://cert-manager.io) is used to manage TLS certificates.

### Sealed secrets

[Sealed secrets](https://github.com/bitnami-labs/sealed-secrets?tab=readme-ov-file) controller is used to safely manage secrets in a GitOps way.
The controller runs in the cluster and decrypts the sealed secrets into regular kubernetes secrets.
The [public key](public-keys/pub-sealed-secrets.pem) can be used to encrypt the secret so that it can safely be stored in git.
The procedure is explained on the FluxCD website [fluxcd.io/flux/guides/sealed-secrets/](https://fluxcd.io/flux/guides/sealed-secrets/)
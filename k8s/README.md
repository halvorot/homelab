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

### MetalLB

[MetalLB](https://metallb.io) is a load balancer controller for bare metal k8s clusters.
It assigns external IP addresses to the services in the cluster with the type LoadBalancer.

My MetalLB controller is deployed in L2 mode because my router does not support BGP.

### Ingress controller

[ingress-nginx](https://github.com/kubernetes/ingress-nginx) is used as the Ingress controller for Kubernetes.
This acts as a reverse proxy and load balancer.

### Cert manager

[cert-manager](https://cert-manager.io) is used to manage TLS certificates for the ingresses.

### Cloudflare Tunnel
A [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) is used to expose services online.
The tunnel client runs inside the cluster and routes traffic from cloudflare to the Ingress NGINX Controller.

### Sealed secrets

[Sealed secrets](https://github.com/bitnami-labs/sealed-secrets?tab=readme-ov-file) controller is used to safely manage secrets in a GitOps way.
The controller runs in the cluster and decrypts the sealed secrets into regular kubernetes secrets.
The [public key](public-keys/pub-sealed-secrets.pem) can be used to encrypt the secret so that it can safely be stored in git.
The procedure is explained on the FluxCD website [fluxcd.io/flux/guides/sealed-secrets/](https://fluxcd.io/flux/guides/sealed-secrets/)

### Longhorn

[Longhorn](https://longhorn.io) is used as the storage provider to enable provisioning of persistent volume claims.
Longhorn supports high availability and includes functionality for snapshots and backups.

### MinIO

[MinIO](https://min.io) is used for S3 compatible bucket storage.

### Kube-Prometheus-Stack

The [Kube-Prometheus-Stack](https://github.com/prometheus-operator/kube-prometheus) is used for monitoring.
The stack contains Grafana, Prometheus and AlertManager as a robust monitoring system for k8s.
# K8S

## Cluster

[MicroK8s](https://microk8s.io) is used for a lightweight kubernetes cluster.
This is deployed on a Ubuntu Server VM in Proxmox VE.

## Continuous deployment - GitOps

The kubernetes cluster is set up with [FluxCD](https://fluxcd.io) for continuous deployment.

Flux uses GitOps practices and a pull-based deployment model.

The deployments are structured in a monorepo approach, see [Ways of structuring your repositories](https://fluxcd.io/flux/guides/repository-structure/).
Deployment manifests for all apps and services in the cluster are defined here in this repository for Flux to sync and deploy.


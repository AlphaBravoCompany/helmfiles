# Development Deployment
Automated deployment using Make.

## Requirements
* [All Helmfile requirements](../README.md)
* Make
* [Kubectl](https://kubernetes.io/docs/tasks/tools/) >= 1.22
* [K3d](https://k3d.io/v5.3.0/) >= v5.3.0

## Secret Decryption
Before running the Make, be sure to update the `SOPS_AGE_KEY_FILE` environment variable, as noted in the [root level README file](../README.md) under the **Decrypting Secrets** section..

## Make File Options
| Commands | Description |
| --- | --- |
| helm-install | Deploys the Helm chart(s) for ABScan. To specify a specific deployment environment, set the ENVIRONMENT variable. <br><br> Example: <br> `make helm-install ENVIRONMENT=staging` |
| helm-uninstall | Removes the Helm deployment created by the `helm-install` command. <br> **CRITICAL**: This is a DESTRUCTIVE action. |
| k3d-install | Deploys a local K3d cluster with load balancer. Default `agents` is `1`. To update agent count, set the AGENT variable. <br><br> Example: <br> `make k3d-install AGENT=3` <br><br> **NOTE**: This deployment will automatically change your `kubectl` context to `k3d-abscan-dev`. |
| k3d-uninstall | Deletes the local K3d cluster created by the `k3d-install` command. <br> **CRITICAL**: This is a DESTRUCTIVE action. |
| setup | Deploys a K3d cluster and installs the development Helm charts. |

## Kubernetes Management
For managing the development Kubernetes cluster, an optional step is to [install Lens Desktop](https://k8slens.dev/).
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

### Default variables
Environment variables can be added to any make command. The defaults are:
* **CLUSTERNAME** = abscan-dev
* **AGENT** = 1
* **ENVIRONMENT** = default
* **HOSTPORT** = 80
* **SERVICEPORT** = 80
* **SECUREHOSTPORT** = 443
* **SECURESERVICEPORT** = 443
* **DNSNAME** = abscan.io
* **NAMESPACE** = abscan

### Commands

| Commands | Description |
| --- | --- |
| helm-install | Deploys the Helm chart(s) for ABScan. <br><br> Optional variables: <br> - **ENVIRONMENT** <br><br> Example: <br> `make helm-install ENVIRONMENT=staging` |
| helm-uninstall | Removes the Helm deployment created by the `helm-install` command. <br> **CRITICAL**: This is a DESTRUCTIVE action. |
| k3d-install | Deploys a local K3d cluster with load balancer. <br><br> Optional variables: <br> - **CLUSTERNAME** <br> - **AGENT** <br> - **HOSTPORT** <br> - **SERVICEPORT** <br> - **SECUREHOSTPORT** <br> - **SECURESERVICEPORT** <br> - **DNSNAME** <br><br> Examples: <br> `make k3d-install AGENT=3` <br> `make k3d-install AGENT=5 HOSTPORT=8080` <br> `make k3d-install HOSTPORT=8080 SERVICEPORT=81` <br><br> **NOTE**: By default, the deployment will automatically change your `kubectl` context to `k3d-abscan-dev`. If you use the `CLUSTERNAME` variable, the context will become `k3d-<CLUSTERNAME>` instead. |
| k3d-uninstall | Deletes the local K3d cluster created by the `k3d-install` command. <br><br> **NOTE**: If the `CLUSTERNAME` varible is used, when creating the cluster, you must use the same variable when running this command. <br><br> **CRITICAL**: This is a DESTRUCTIVE action. |
| setup | Deploys a K3d cluster and installs the development Helm charts. <br><br> Optional variables: <br> - **ALL** <br><br> Examples: <br> `make setup ENVIRONMENT=default DNSNAME=abscan.dev` <br> `make setup HOSTPORT=8080` <br> `make setup AGENT=3` |
| setup-uninstall | Removes the development environment. <br><br> **NOTE**: If the `CLUSTERNAME` varible is used, when creating the cluster, you must use the same variable when running this command. <br><br> **CRITICAL**: This is a DESTRUCTIVE action. |

## Kubernetes Management
For managing the development Kubernetes cluster, an optional step is to [install Lens Desktop](https://k8slens.dev/).

## Troubleshooting
**Error:** `Bind for 0.0.0.0:80 failed: port is already allocated` <br>
**Solution:** A K3d deployment or other service using port 80 on your local environment. Stop this service and try again.
<br><br>
**Error:** The K3d deployment hangs with the Docker log showing `unknown service runtime.v1alpha2.RuntimeService`. <br>
**Solution:** Remove any older running K3d clusters and try again.
# irsa-manager-flux-demo

This repository is designed to facilitate the GitOps approach for managing Kubernetes clusters and associated AWS resources. It leverages Flux for continuous deployment and includes configurations for setting up Amazon Controllers for Kubernetes (ACK) and IAM Roles for Service Accounts (IRSA) using the IRSA Manager. The structured folder layout helps in organizing infrastructure, application deployments, and initial setups, making it easier to manage and automate Kubernetes operations in a cloud-native environment.

## folder structure

```
.
├── application
│   ├── base/
│   ├── eks/
│   └── selfhosted/
├── clusters
│   ├── eks/
│   └── selfhosted/
├── infrastructure
│   ├── base/
│   ├── eks/
│   └── selfhosted/
└── init
    ├── base/
    ├── eks/
    └── selfhosted/

```

- clusters: The entry point for GitOps configurations.
- infrastructure: Contains ACK (Amazon Controllers for Kubernetes) and IRSA (IAM Roles for Service Accounts) settings.
- application: Defines AWS resources.
- init: Initializes the IRSA Manager.


## Procedure


Follow these steps to activate the ACK (Amazon Controller for Kubernetes) using the IRSA Manager:

1. Set `irsa-setup` Configuration
  * **For Self-Hosted Kubernetes**: Update the `spec.discovery.s3.region` and `spec.discovery.s3.bucketName` fields in the `infrastructure/selfhosted/irsa-setup.yaml` file.
  * **For EKS**: Update the `spec.iamOIDCProvider` fields in the `infrastructure/eks/irsa-setup.yaml` file.

2. Run the [Flux bootstrap](https://fluxcd.io/flux/cmd/flux_bootstrap_github/) command
  * **For Self-Hosted Kubernetes**: Use the `clusters/selfhosted` directory.
  * **For EKS**: Use the `clusters/eks` directory.

3. (Self-Hosted Only) Setup kube-apiserver for IRSA Manager. 
  * Follow the instructions provided in the [IRSA Manager](https://github.com/kkb0318/irsa-manager/blob/main/docs/selfhosted-setup.md).

4. (Self-Hosted Only) Restart ack-controller Deployment to provision the IRSA tokens to the pods. (This step is only needed once after the initial setup to ensure that the tokens are correctly provisioned.)


If you want to create the sample AWS resources, remove the comments in the `application/base/kustomization.yaml` file.

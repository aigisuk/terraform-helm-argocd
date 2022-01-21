# Terraform Helm ArgoCD Module
A Terraform module to deploy [ArgoCD](https://argoproj.github.io/cd/) on a Kubernetes Cluster using the [Helm Provider](https://registry.terraform.io/providers/hashicorp/helm).

![Concept Flow Illustration](https://res.cloudinary.com/qunux/image/upload/v1642563762/terraform_kubernetes_argocd_mtze07.svg)

## Default Admin Password

If the `admin_password` input variable is **not** set, the initial password for the `admin` user account is auto-generated and stored as clear text in the field `password` in a secret named `argocd-initial-admin-secret` in your Argo CD installation namespace. You can simply retrieve this password using `kubectl`[^1]:

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| release_name | Helm release name | string | `argocd` | no |
| namespace | Namespace to install ArgoCD chart into (created if non-existent on target cluster) | string | `argocd` | no |
| argocd_chart_version | Version of ArgoCD chart to install | string | `3.31.0` | no |
| timeout_seconds | Helm chart deployment can sometimes take longer than the default 5 minutes. Set a custom timeout (secs) | number | `800` | no |
| admin_password | Default Admin password | string | empty | no |
| insecure | Disable TLS on the ArogCD API Server? | bool | `false` | no |
| values_file | Name of the ArgoCD helm chart values file to use | string | `values.yaml` | no |


[^1]: [Login Using The CLI](https://argo-cd.readthedocs.io/en/stable/getting_started/#4-login-using-the-cli)
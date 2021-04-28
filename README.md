Terraform ready cert-manager

Usage
-----

```hcl
module "cert_manager" {
  source = "./cert-manager"

  providers = {
    kubernetes = kubernetes
    kubernetes-alpha = kubernetes-alpha
  }
}
```

Building
--------

1) generate with
    ```shell
    tfk8s --strip --file cert-manager_v1.2.0.yaml --output cert-manager_v1.2.0.tf --provider "kubernetes-alpha"
    ```

2) replace
   `"namespace" = "cert-manager"`
   with
   `"namespace" = kubernetes_manifest.namespace_cert_manager.manifest.metadata.name`

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


resource "kubectl_manifest" "cert_manager_cluster_issuer" {
  depends_on = [module.cert_manager]
  yaml_body = <<YAML
    apiVersion: cert-manager.io/v1
    kind: ClusterIssuer
    metadata:
      name: letsencrypt
    spec:
      acme:
        email: my@email
        server: https://acme-v02.api.letsencrypt.org/directory
        preferredChain: "ISRG Root X1"
        privateKeySecretRef:
          name: letsencrypt
        solvers:
          - http01:
              ingress:
                class: nginx
YAML
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

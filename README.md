# README

This role manages helm-charts and custom manifests on k8s cluster

## Prerequisites

- k8s serviceaccount api token

## Usage

### Helm charts

If you would like to deploy helm charts you can create a dictionary in the host_vars in the configuration repository.
When you have custom values, please provide a custom-path from the configuration repository;

> The name of the dictionary is also the name of the instance of the helm-chart


Example:
```yaml
    k8s_manifest_helm_charts:
      cert-manager:
        namespace: cert-manager
        chart_ref: cert-manager
        repo_url: 'https://charts.jetstack.io'
        version: v1.11.0
        value_file: "/home/marijn/projects/homelab/ingress-nginx.yml"
      ingress-nginx:
        namespace: ingress-nginx
        ...
        ...
```

### Manifest files

When you want to deploy manifest files please define the ```k8s_manifest_sourcedir```-variable, this variable needs to resolve to a directory which contains the structure;

```text
namespace/
namespace/01-namespace.yml
namespace/02-xxxx.yml
namespace02/
namespace02/01-namespace.yml
namespace02/02-namespace.yml
```

The code will loop through the defined manifest_sourcedir and picks every first directory (highest-level) as the namespace where he needs to handle the manifest files.

Only files with the **.yml** extension are looped through.

#### Global manifests

In some situations there are manifests that doesn't have a namespace where they operate in. Then you can use the `k8s_manifest_global_sourcedir` variable. 
Default `k8s_manifest_global_sourcedir` is defined as a '_global' folder in the `k8s_manifest_sourcedir`.
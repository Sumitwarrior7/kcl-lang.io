---
title: "Helmfile KCL Plugin"
sidebar_position: 5
---

## Introduction

[Helmfile](https://github.com/helmfile/helmfile) is a declarative spec for deploying helm charts. It lets you...

- Keep a directory of chart value files and maintain changes in version control.
- Apply CI/CD to configuration changes.
- Periodically sync to avoid skew in environments.

KCL can be used to create functions to mutate and/or validate the YAML Kubernetes Resource Model (KRM) input/output format, and we provide Kustomize KCL functions to simplify the function authoring process.

## Prerequisites

- Install [helmfile](https://github.com/helmfile/helmfile)
- Prepare a Kubernetes cluster

## Quick Start

Let’s write a KCL function which add annotation `managed-by=helmfile-kcl` only to Deployment resources.

### 1. Get the Example

```bash
git clone https://github.com/kcl-lang/helmfile-kcl.git
cd ./helmfile-kcl/examples/hello-world/
```

We can execute the command to show config

```bash
cat helmfile.yaml
```

The output is

```yaml
repositories:
  - name: prometheus-community
    url: https://prometheus-community.github.io/helm-charts

releases:
  - name: prom-norbac-ubuntu
    namespace: prometheus
    chart: prometheus-community/prometheus
    set:
      - name: rbac.create
        value: false
    transformers:
      # Use KCL Plugin to mutate or validate Kubernetes manifests.
      - apiVersion: krm.kcl.dev/v1alpha1
        kind: KCLRun
        metadata:
          name: "set-annotation"
          annotations:
            config.kubernetes.io/function: |
              container:
                image: docker.io/kcllang/kustomize-kcl:v0.2.0
        spec:
          source: |
            [resource | {if resource.kind == "Deployment": metadata.annotations: {"managed-by" = "helmfile-kcl"}} for resource in option("resource_list").items]
```

In the above config, we use a `KCLRun` plugin to assign the `transfomer` field. This means that we will add annotations to all deployment resources in the prometheus helm chart.

### 2. Test and Run

Firstly, init the helmfile tool.

```bash
helmfile init
```

The output may looks like this:

```bash
The helm plugin helm-git is not installed, do you need to install it [y/n]: y
Install helm plugin helm-git
Installed plugin: helm-git

helmfile initialization completed!
...
```

Then apply the configuration.

```bash
helmfile apply
```

The output is

```bash
Adding repo prometheus-community https://prometheus-community.github.io/helm-charts
"prometheus-community" has been added to your repositories

...
```

## Guides for Developing KCL

Here's what you can do in the KCL code:

- Read resources from `option("items")`. The `option("items")` complies with the [KRM Functions Specification](https://kpt.dev/book/05-developing-functions/01-functions-specification).
- Return a KRM list for output resources.
- Return an error using `assert {condition}, {error_message}`.
- Read the PATH variables. e.g. `option("PATH")`.
- Read the environment variables. e.g. `option("env")`.

## More Documents and Examples

- [KRM KCL Spec](https://github.com/kcl-lang/krm-kcl)

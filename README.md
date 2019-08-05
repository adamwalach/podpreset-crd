# PodPresets CRD

The source in the repository contains a PodPreset resource that's implemented
via a CRD that is paired with a mutating webhook in order to modify pod specs
on the fly.

## Getting started

In order to deploy the PodPreset resource into your cluster, do the following:

1. Deploy Kubernetes.

1. Install [kustomize](https://github.com/kubernetes-sigs/kustomize).

1. Run CRD, choose to execute inside or outside the cluster.

   Inside cluster:

   ```shell
   make docker-build
   make deploy
   ```

   Outside cluster (for debugging/development):

   ```shell
   make install
   make run
   ```

1. Build the webhook container image.

   ```shell
   make build-webhook
   make build-image-webhook
   ```

1. Install the webhook.

   ```shell
   make deploy-webhook
   ```

1. Apply desired pod presets as needed, example given below.

## Build process

Docker images for controller and webhook server are automatically created on every commit and PR:
- eu.gcr.io/kyma-project/incubator/pr/podpreset-controller:PR-X
- eu.gcr.io/kyma-project/incubator/pr/podpreset-webhook:PR-X

## Example usage

```shell
kubectl create -f config/samples/apod-preset.yaml
kubectl create -f config/samples/apod.yaml
```

## History

PodPresets has had a long history in various forms throughout its alpha life
(it's still alpha). As of August 2018, the PodPreset resource still exists in
the Kubernetes core, but is planned to be superceded by the code here.

## Additional information

### Certificates

The Makefile utilizes pre-generated certificates for use with the mutating
webhook. An example script that generates new certificates is located in
[webhook/pki/gen-certs.sh](webhook/pki/gen-certs.sh). The [cabundle patch file](webhook/kustomize-config/webhook_cabundle_patch.yaml)
needs to be adjusted accordingly afterwards, unless the deployment file is used directly.

### Service Catalog integration

This repository is only one piece required for full integration with Service
Catalog. Also review the [podpresetbinding-crd](https://github.com/jpeeler/podpresetbinding-crd)
repository for functionality that auto-generates pod presets upon service
bindings being ready.

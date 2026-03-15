# k8s-argo-workflows

You can pull the chart from its official repo:
```bash
# add the repo
helm repo add argo https://argoproj.github.io/argo-helm
# search available charts
helm search repo argo-workflows --versions
# install
helm upgrade --install argo-workflows argo/argo-workflows --namespace argo --create-namespace -f values.yaml
# or pull a specific version
helm pull argo-workflows --repo https://argoproj.github.io/argo-helm --version 1.0.2 --untar
```
This repo contains a parent helm chart, and is initialized with: 
```bash
mkdir k8s-argo-workflows
cd k8s-argo-workflows
helm create helm
rm -rf helm/templates/*
```
Edit helm/Chart.yaml:
```Bash
apiVersion: v2
name: argo-workflows
description: A Helm chart for Argo Workflows on Kubernetes

type: application
version: 0.1.0
appVersion: "v4.0.2"

dependencies:
  - name: argo-workflows
    version: 1.0.2 # version of the helm chart for argo-workflows used
    repository: https://argoproj.github.io/argo-helm
```
Run the dependency update which will download the Argo Workflows helm chart in tgz format and place it in the charts sub-folder as a child helm chart. It will also create the Chart.lock file.
```Bash
cd helm
helm dependency update
cd charts
tar xvf *tgz
rm -f *tgz
```
Next, edit the values.yaml file in the parent helm chart, e.g. copy the values.yaml file from the child helm chart and update the chart where necessary. It is best practice to only put those items in the parent helm chart values.yaml file that have modified; not the entire helm chart copy.

Also, the entrie values.yaml file needs to be indented 2 spaces underneath the name of the child helm chart, e.g.:
```Bash
$ cat values.yaml
argo-workflows:
  server:
    # Set authmode to server
    authModes:
    - server
...
```

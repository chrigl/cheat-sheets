---
tags: [k8s, kubernetes, kubectl]
---
# Get number of attached volumes per node

```bash
kubectl get nodes -ogo-template='{{range .items}}{{ if .status.volumesAttached }}{{.metadata.name}} - {{.status.volumesAttached | len}}{{"\n"}}{{end}}{{end}}'
```

Note: This only works in non-[CSI](https://github.com/container-storage-interface/spec) environments.

# Decode secrets

```bash
kubectl get secret my-secret -ogo-template='{{index .data "config.yaml" | base64decode}}'
```

# Run `kubectl` as serviceaccount

```bash
kubectl -n kube-public --as system:serviceaccount:monitoring:prometheus-operator get prometheusrules
```

# Run a debug DaemonSet

see gist runs in kube-system [debug-ds.yaml](https://gist.github.com/chrigl/6184d4de911052711b149665829ce66d)

```bash
kubectl apply -f https://gist.githubusercontent.com/chrigl/6184d4de911052711b149665829ce66d/raw/9bdad211a6bdc6498d0722614b2e1911224bdbf5/debug-ds.yaml
```

# Get logs of previous terminated container

```bash
kubectl logs -p -c ruby web-1
```

# Find all jobs in BackLimitExceeded

No error handling here, so be prepared for `jq: error (at <stdin>:17450): Cannot iterate over null (null)`

```bash
kubectl  get jobs -ojson -A | jq -r '.items[] | select(.status.conditions[] | select(.reason=="BackoffLimitExceeded" and .type == "Failed" and .status == "True")) | [.metadata.namespace, .metadata.name] | @tsv'
```

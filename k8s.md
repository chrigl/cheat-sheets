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

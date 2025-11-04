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
kubectl apply -f https://gist.githubusercontent.com/chrigl/6184d4de911052711b149665829ce66d/raw/11ec5a0196fd0c8f415e7a3faf53e68e523d66e4/debug-ds.yaml
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

# ingress nginx

Hmm. Maybe not the right place here, but good for now.

`ingress-nginx` doesn't write the the full configuration to `/etc/nginx/nginx.conf` but uses lua for some parts. There is a tool `/dbg` in the container to help debugging.

```bash
$ /dbg --help
dbg is a tool for quickly inspecting the state of the nginx instance

Usage:
  dbg [command]

Available Commands:
  backends    Inspect the dynamically-loaded backends information
  certs       Inspect dynamic SSL certificates
  completion  Generate the autocompletion script for the specified shell
  conf        Dump the contents of /etc/nginx/nginx.conf
  general     Output the general dynamic lua state
  help        Help about any command

Flags:
  -h, --help              help for dbg
      --status-port int   Port to use for the lua HTTP endpoint configuration. (default 10246)

Use "dbg [command] --help" for more information about a command.
```

# Split resource list into a multi document yaml

```bash
kubectl -n cortex get ingress -oyaml | yq e '.items[] | split_doc'
```

# Debug node

```bash
kubectl debug node/kind-control-plane --image nicolaka/netshoot:latest -ti -- /bin/bash
```

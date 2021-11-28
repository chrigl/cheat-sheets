---
tags: [curl]
---
# Measure time until first byte received

```
curl -o /dev/null -s -k https://192.168.1.12:10250/metrics -w "%{time_starttransfer}\n"
```

# Present a client certificate

e.g. accessing metrics endpoint of etcd

```
curl --cert tls.crt --key tls.key -s -k https://etcd-1.etcd.cluster-vzkhcmnnt4.svc.cluster.local:2379/metrics
```

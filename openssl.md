# Create a CSR with a new key and sign it with a CA

This is only useful if the CA is at hand, but useful for some internal bootstrapping of certs.

The example signs a certificate to access the metrics endpoint of etcd.
```
openssl req -new -newkey rsa:2048 -nodes -keyout etcd-healthz.key -subj /CN=etcd-healthz -out etcd-healthz.csr
openssl x509 -req -in etcd-healthz.csr -days 3600 -CAcreateserial -CA /etc/ssl/etcd/ssl/ca.pem -CAkey /etc/ssl/etcd/ssl/ca-key.pem  -out etcd-healthz.crt
```

# Get server certificate

```
echo | openssl s_client -connect heise.de:443 -showcerts | openssl x509 -noout -text
```

---
tags: [iproute2, ip, vxlan]
---

# VXLAN multicast

```bash
ip link add bgptun type vxlan id 100 dstport 4789 group 239.1.1.1 dev ens3 local 192.168.1.107
```

# VXLAN direct

```bash
ip link add bgptun type vxlan id 100 dstport 4789 dev ens3 local 192.168.1.107 remote REMOTE_IP
```

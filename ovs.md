---
tags: [networking]
---

# Create a port

```
ovs-vsctl add-port br-ex myfoo -- set Interface myfoo type=internal
```

# Create a trace packet

```
ovs-appctl -t /var/run/openvswitch/ovs-vswitchd.6.ctl ofproto/trace br-int in_port=qvo09952d67-57,dl_src=fa:16:3e:aa:66:67


Flow: in_port=1364,vlan_tci=0x0000,dl_src=fa:16:3e:aa:66:67,dl_dst=00:00:00:00:00:00,dl_type=0x0000
bridge("br-int")
----------------
 0. in_port=1364, priority 9, cookie 0x1c619db8cc58621e
    goto_table:25
25. in_port=1364,dl_src=fa:16:3e:aa:66:67, priority 2, cookie 0x1c619db8cc58621e
    goto_table:60
60. priority 3, cookie 0x1c619db8cc58621e
    NORMAL
     -> no learned MAC for destination, flooding
bridge("br-tun")
----------------
 0. in_port=1, priority 1, cookie 0x1d6f8f2ef195cc3
    goto_table:2
 2. dl_dst=00:00:00:00:00:00/01:00:00:00:00:00, priority 0, cookie 0x1d6f8f2ef195cc3
    goto_table:20
20. priority 0, cookie 0x1d6f8f2ef195cc3
    goto_table:22
22. dl_vlan=884, priority 1, cookie 0x1d6f8f2ef195cc3
    pop_vlan
    set_field:0x4a->tun_id
    output:3
     -> output to kernel tunnel
    output:2
     -> output to kernel tunnel
    output:4
     -> output to kernel tunnel
    output:7
     -> output to kernel tunnel
    output:5
     -> output to kernel tunnel
    output:6
     -> output to kernel tunnel
    output:9
     -> output to kernel tunnel
    output:10
     -> output to kernel tunnel
    output:8
     -> output to kernel tunnel
Final flow: unchanged
Megaflow: recirc_id=0,eth,in_port=1364,vlan_tci=0x0000,dl_src=fa:16:3e:aa:66:67,dl_dst=00:00:00:00:00:00,dl_type=0x0000
Datapath actions: push_vlan(vid=884,pcp=0),1,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.13,ttl=64,tp_dst=4789,flags(df|key))),pop_vlan,3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.14,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.8,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.6,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.7,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.5,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.2,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.3,ttl=64,tp_dst=4789,flags(df|key))),3,set(tunnel(tun_id=0x4a,src=10.91.2.12,dst=10.91.2.1,ttl=64,tp_dst=4789,flags(df|key))),3,30,31,32,33,40,41,42,57,60,61
```

# Get details of protocol

```
[root@server0 ~]# birdc show protocol all eth2
BIRD 2.0.7 ready.
Name       Proto      Table      State  Since         Info
eth2       BGP        ---        up     2020-10-26    Established
  BGP state:          Established
    Neighbor address: 10.3.0.1
    Neighbor AS:      4200000012
    Local AS:         4200000011
    Neighbor ID:      10.64.64.1
    Local capabilities
      Multiprotocol
        AF announced: ipv4
      Route refresh
      Graceful restart
        Restart time: 30
        AF supported: ipv4
        AF preserved:
      4-octet AS numbers
      Enhanced refresh
      Long-lived graceful restart
    Neighbor capabilities
      Multiprotocol
        AF announced: ipv4
      Route refresh
      Graceful restart
      4-octet AS numbers
      ADD-PATH
        RX: ipv4
        TX:
    Session:          external AS4
    Source address:   10.3.0.2
    Hold timer:       5.394/9
    Keepalive timer:  0.649/3
  Channel ipv4
    State:          UP
    Table:          master4
    Preference:     100
    Input filter:   import_fabric
    Output filter:  export_fabric
    Routes:         3 imported, 3 exported, 0 preferred
    Route change stats:     received   rejected   filtered    ignored   accepted
      Import updates:              3          0          0          0          3
      Import withdraws:            0          0        ---          0          0
      Export updates:             12          0          9        ---          3
      Export withdraws:            0        ---        ---        ---          0
    BGP Next hop:   10.3.0.2
```

# List exported routes of protocol

```
[root@server0 ~]# birdc show route export eth2
BIRD 2.0.7 ready.
Table master4:
10.64.144.1/32       unicast [direct1 2020-10-26] * (240)
        dev kube-ipvs0
10.64.64.2/32        unicast [direct1 2020-10-26] * (240)
        dev fabric
10.64.146.247/32     unicast [direct1 2020-10-26] * (240)
        dev kube-ipvs0
```

# In case of emergency: Talk directly to bird (without birdc)

NOTE: you must insert the correct wire-protocol command. E.g. the well known `show protocol` or `show proto` won't work.

```
[root@server0 ~]# socat - unix-connect:/run/bird/bird.ctl
0001 BIRD 2.0.7 ready.
show protocols
2002-Name       Proto      Table      State  Since         Info
1002-direct1    Direct     ---        up     2020-10-26
 bfd1       BFD        ---        up     2020-10-26
 kernel1    Kernel     master4    up     2020-10-26
 device1    Device     ---        up     2020-10-26
 eth1       BGP        ---        up     2020-10-26    Established
 eth2       BGP        ---        up     2020-10-26    Established
0000
show status
1000-BIRD 2.0.7
1011-Router ID is 10.64.64.2
 Current server time is 2020-10-27 14:16:51.409
 Last reboot on 2020-10-26 17:08:42.880
 Last reconfiguration on 2020-10-26 17:08:42.880
```

# What does the output mean

```
# birdc show route
BIRD 1.6.8 ready.
0.0.0.0/0          via 100.64.0.13 on ens1f0 [ens1f0 13:07:19] ! (100) [AS65400i]
                   via 100.64.1.13 on ens1f1 [ens1f1 13:07:19] (100) [AS65400i]
                   via 10.40.83.45 on ens6f0 [kernel1 13:07:17] (10)
10.131.0.0/23      dev tun0 [direct1 13:07:34] * (240)
10.0.0.0/8         via 100.64.0.13 on ens1f0 [ens1f0 13:07:19] * (100) [AS65400i]
                   via 100.64.1.13 on ens1f1 [ens1f1 13:07:20] (100) [AS65400i]
```

```
*    - Selected route
!    - Unable to push route... combined with an Invalid argument in birds log
```

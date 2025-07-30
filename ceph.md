---
tags: [ceph]
---
# TheJJ/ceph-cheatsheet

[The definitive ceph cheat sheet is here](https://github.com/TheJJ/ceph-cheatsheet)

# Set cpu governor to `performance` on OSDs

OSDs need all the power they can get.
While this certainly comes at a cost of more power consumption and more cooling, in cuts write latencies by roughly a third on modern Xeon processors.

Sometimes you can change it on OS level, but I also saw servers where adjustments to bios settings were needed.

From my experience, this recommendation also applies to [Quobyte Storage](https://www.quobyte.com/).

# Allow deletion of pools

```
ceph tell "mon.*" config set mon_allow_pool_delete true
```

# Change default crush rule

```
ceph config get global osd_pool_default_crush_rule $ID
```

You might want to do this in a setup with mixed disk types.

# Notes

## Subvolume snapshot mirroring

* https://tracker.ceph.com/issues/49125
* https://github.com/ceph/ceph/pull/42091

# Get front / back ip addresses and more info

```
ceph osd metadata [osd.ID]
```

# extra-modules / vrf

Some modules are not there even though they are listed in the config e.g. `vrf`. This happend in the official focal cloud image.

```
# ip link add red type vrf table 1
Error: Unknown device type.

# modprobe vrf
modprobe: FATAL: Module vrf not found in directory /lib/modules/5.4.0-48-generic

# grep VRF /boot/config-5.4.0-*
/boot/config-5.4.0-48-generic:CONFIG_NET_VRF=m

# uname -r
5.4.0-48-generic
```

In this case `linux-modules-extra-5.4.0-48-generic` is missing. To get the `extra-modules` for kernel updates you *must* install `linux-image-generic`, while in VMs only `linux-image-virtual` is installed.

```
apt install linux-image-generic
```

```
Package: linux-image-generic
Version: 5.4.0.65.68
Priority: optional
Section: kernel
Source: linux-meta
Origin: Ubuntu
Maintainer: Ubuntu Kernel Team <kernel-team@lists.ubuntu.com>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 18.4 kB
Provides: virtualbox-guest-modules (= 6.1.10-dfsg-1~ubuntu1.20.04.1), wireguard-modules (= 1.0.20201112-1~20.04.1), zfs-modules (= 0.8.3-1ubuntu12.5)
Depends: linux-image-5.4.0-65-generic, linux-modules-extra-5.4.0-65-generic, linux-firmware, intel-microcode, amd64-microcode
Recommends: thermald
Download-Size: 2620 B
APT-Manual-Installed: no
APT-Sources: http://es1.clouds.archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages
Description: Generic Linux kernel image
 This package will always depend on the latest generic kernel image
 available.

N: There is 1 additional record. Please use the '-a' switch to see it
```

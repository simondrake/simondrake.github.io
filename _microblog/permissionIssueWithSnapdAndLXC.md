---
layout: post
title: Permission errors using snapd in LXC containers
subtitle: Learn how to fix permissions issues when using snapd in an LXC container
date: 2021-04-28
tags: [proxmox]
---

When trying to run `snapd` in an LXC container, you may get the following error:

```
error: system does not fully support snapd: cannot mount squashfs image using "squashfs": mount:
       /tmp/sanity-mountpoint-517447097: mount failed: Operation not permitted.
```

## For unprivileged containers:

* Update `/etc/pve/lxc/$vmid.conf` and add the following two lines


```
...
features: mount=fuse,nesting=1
lxc.mount.entry = /dev/fuse dev/fuse none bind,create=file 0 0
```

* Reboot the container
* Inside the container run `apt install snapd`

## For priviledged containers, also add:

```
# EDIT:
# We need to allow apparmor administration, by default mac_admin is dropped for privileged containers.
# Note that you do not want this for un-trusted containers...
lxc.cap.drop =
lxc.cap.drop = mac_override sys_time sys_module sys_rawio
```

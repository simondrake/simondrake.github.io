---
layout: post
title: Connect to a NFS Mount
subtitle: Learn how to connect to a NFS mount
date: 2021-05-02
tags: [DevOps, Linux]
---

## Mount

* Install the necessary dependencies

<pre class="command-line"><code class="language-bash">
sudo apt install nfs-kernel-server
</code></pre>

* Start the NFS Kernel

<pre class="command-line"><code class="language-bash">
sudo /etc/init.d/nfs-kernel-server start
</code></pre>

* Find the NFS mount by destination IP

<pre class="command-line"><code class="language-bash">
sudo showmount -e {Destination IP}
</code></pre>

* Make the target directory

<pre class="command-line"><code class="language-bash">
sudo mkdir /target
</code></pre>

* Mount the NFS share to the target directory

<pre class="command-line"><code class="language-bash">
sudo mount -t nfs {Destination IP}:{NFS Share from the 'Find the NFS Mount' step} /target/
</code></pre>

* Check it has mounted properly

<pre class="command-line"><code class="language-bash">
df
</code></pre>

## Unmount

<pre class="command-line"><code class="language-bash">
sudo umount -f -l /target
</code></pre>

## Preserver after reboot

* Open fstab

<pre class="command-line"><code class="language-bash">
sudo vi /etc/fstab
</code></pre>

* Add the necessary config

<pre class="command-line"><code class="language-bash">
{Destination IP}:{NFS Share from the 'Find the NFS Mount' step} /target/ nfs auto,nofail,noatime,nolock,intr,tcp,actimeo=1800 0 0
</code></pre>

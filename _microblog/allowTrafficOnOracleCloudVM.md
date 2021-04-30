---
layout: post
title: Allow traffic on Oracle Cloud VM
subtitle: Learn how to allow traffic to a Orcle Cloud VM
date: 2021-04-30
tags: [DevOps]
---

## Oracle Cloud UI
* Allow port in the Security List associated with the Internet Gateway (by default you only have access to SSH and ICMP 3,4 type)
* Allow port in the Network Security Group

## On the Server
* Allow port through `iptables`

<pre class="command-line" data-output="2"><code class="language-bash">
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport {PORT} -j ACCEPT

sudo netfilter-persistent save
</code></pre>


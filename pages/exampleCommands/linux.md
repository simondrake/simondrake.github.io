---
layout: page
title: Linux
subtitle: Linux is a family of open-source Unix-like operating systems based on the Linux kernel
permalink: /example-commands/linux/
---

## Heredoc

The string after << indicates where to stop

{:.language-bash}
```
cat << EndOfMessage
This is line 1.
This is line 2.
Line 3.
EndOfMessage
```

To send these lines to a file, use:

{:.language-bash}
```
cat > $FILE <<- EOM
Line 1.
Line 2.
EOM
```

<br>


# SCP/SSH

### SCP to Server
SCP all files in `/path/to/files/` to `10.10.10.10` to `serverDIR`

<pre><code class="command-line language-bash">
scp -r /path/to/files/* 10.10.10.10:./[serverDIR]
</code></pre>



### SCP From Server
SCP all files from `serverDIR` on `10.10.10.10` to the current local directory

<pre><code class="command-line language-bash">
scp 10.10.10.10:./[serverDIR]/* .
</code></pre>


### SSH Tunnels
Create a SSH tunnel that uses port `9925` on the localhost. When traffic is received on `localhost:9925` it will route it to `10.10.10.10:25`

<pre><code class="command-line language-bash">
ssh -L 9925:localhost:25 10.10.10.10
</code></pre>


Create a SSH tunnel that uses port `8282` and `9282` on the localhost. When traffic is received on `localhost:8282` it will route it to `10.10.10.10:8280` and when traffic is received on `localhost:9282` it will route it to `10.10.10.10:9280`

<pre><code class="command-line language-bash">
ssh -L 8282:localhost:8280 -L 9282:localhost:9280 10.10.10.10
</code></pre>


Create SSH tunnel that when a connection is made to the port `5044` on `10.10.10.10` it should be forwarded to port `5044` on your local machine.

<pre><code class="command-line language-bash">
ssh -R 5044:localhost:5044 10.10.10.10
</code></pre>



### Copy contents of a remote file into clipboard, using `pbcopy`

<pre><code class="command-line language-bash">
ssh -i ~/path/to/key ubuntu@[IP] "cat [path-to-file]" | pbcopy
</code></pre>

<br>

## Symbolic-Links
Create a symbolic link called `myApplication` that is linked to `myApplication_1.0.3`

<pre><code class="command-line language-bash">
ln -s myApplication_1.0.3 myApplication
</code></pre>


<br>

# Exploratory Commands

### Find processes running on specific port

<pre><code class="command-line language-bash">
sudo lsof -i tcp:port
</code></pre>


### Find biggest 30 files

<pre><code class="command-line language-bash">
sudo find -type f -exec du -Sh {} + | sort -rh | head -n 30
</code></pre>


### Find open file handles, by pid and name

<pre><code class="command-line language-bash">
lsof | awk '{ print $2 " " $1; }' | sort -rn | uniq -c | sort -rn | head -20
</code></pre>

<br>

# NC

<pre><code class="command-line language-bash">
nc -z -v {host} {port}
</code></pre>

<br>

# Kill commands

## Kill multiple processes

<pre><code class="command-line language-bash">
for pid in $(ps -ef | grep "process name" | awk '{print $2}'); do kill -9 $pid; done
</code></pre>

<pre><code class="command-line language-bash">
pkill -9 {process name}
</code></pre>

<br>

# Remove from Systemd

<pre><code class="command-line language-bash">
systemctl stop [servicename]
systemctl disable [servicename]
rm /etc/systemd/system/[servicename]
rm /etc/systemd/system/[servicename] symlinks that might be related
systemctl daemon-reload
systemctl reset-failed
</code></pre>

<br>

# iptables

Redirect port `80` to `9999`

<pre><code class="command-line language-bash">
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 9999
</code></pre>


Redirect port `443` to `9991`

<pre><code class="command-line language-bash">
iptables -t nat -A PREROUTING -p tcp -m tcp --dport 443 -j REDIRECT --to-ports 9991
</code></pre>


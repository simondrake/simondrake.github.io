---
layout: post
title: Run Postfacto on any Cloud Provider
subtitle: Learn how to install/run Postfacto on any Cloud Provider
date: 2021-04-30
tags: [DevOps]
---

[Postfacto](https://github.com/pivotal/postfacto) is an excellent tool for Sprint Retrospectives, but the deployment instructions are limited to four platforms (at the time of writing).

If you only need a simple installation, it's entirely possible to get Postfacto running on any Cloud Provider, and using Docker.

## What you'll need

To run through these steps, you'll need to have a Virtual Machine already provisioned and running Linux. I'll be using Ubuntu 20.04, but most of the commands should work across distros.

## Install Docker

**Note:** it's always better to reference the [official documentation](https://docs.docker.com/engine/install/ubuntu/) but the steps, which are correct at the time of writing, have been copied here for ease.

* Update the apt package index and install packages to allow apt to use a repository over HTTPS:

<pre class="command-line" data-output="2,4,5,6,7,8"><code class="language-bash">
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
</code></pre>

* Add Docker’s official GPG key:

<pre class="command-line"><code class="language-bash">
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
</code></pre>

* Set up the stable repository.

<pre class="command-line" data-output="2,3"><code class="language-bash">
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
</code></pre>

* Update the apt package index, and install the latest version of Docker Engine and containerd

<pre class="command-line" data-output="2"><code class="language-bash">
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io
</code></pre>

* Add your user to the docker group

<pre class="command-line"><code class="language-bash">
sudo usermod -aG docker ${USER}
</code></pre>

* Log in and out of the VM

* Confirm that your user is now added to the docker group (should output `docker`)

<pre class="command-line"><code class="language-bash">
id -nG
</code></pre>

* Install `docker-compose`

<pre class="command-line" data-output="2"><code class="language-bash">
sudo apt update

sudo apt install docker-compose
</code></pre>

* Check installation was successful

<pre class="command-line"><code class="language-bash">
docker-compose --version
</code></pre>

## Set-up Postfacto

**Note:** Make sure to replace all passwords with more secure values.

* Create a docker-compose file with the following contents

{: .language-yaml}
```
version: "3.3"
services:
    postgres:
        image: postgres
        environment:
            - POSTGRES_PASSWORD=example
    postfacto:
        image: postfacto/postfacto
        ports:
            - "3000:3000"
        environment:
            - DATABASE_URL=postgresql://postgres:example@postgres:5432/postgres
            - DISABLE_SSL_REDIRECT=true
            - SECRET_KEY_BASE=iamasecret
            - USE_POSTGRES_FOR_ACTION_CABLE=true
        links:
            - postgres
```

* Start the containers

<pre class="command-line"><code class="language-bash">
docker-compose up -d
</code></pre>

* Register an admin user

<pre class="command-line"><code class="language-bash">
docker-compose exec postfacto create-admin-user {email} {password}
</code></pre>

### Create a retro

* Go to `http://{server-ip}:3000/admin/` and login using the email/password from the last step
* Go to the `Retros` tab and create a retro with the required configuration
* Use your new retro at `http://{server-ip}:3000/retros/{retro-slug}`

## Install/Setup Nginx Reverse Proxy

**Note:** This step is optional, and is only required if you want to use a domain name instead of the Public IP to use Postfacto.

* Install Nginx

<pre class="command-line"><code class="language-bash">
sudo apt-get install nginx
</code></pre>

* Create the postfacto configuration under `/etc/nginx/sites-available/postfacto`, with the follow contents:

{: .language-yaml}
```
server {
        listen 80;
        listen [::]:80;

        server_name <yourdomain(s)>;

        location / {
                proxy_pass http://localhost:3000;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header Host $host;
                proxy_set_header X-Forwarded-Proto $scheme;
        }
}
```

* Enable the configuration

<pre class="command-line"><code class="language-bash">
sudo ln -s /etc/nginx/sites-available/postfacto /etc/nginx/sites-enabled/
</code></pre>


* Test the config & Restart nginx

<pre class="command-line"><code class="language-bash">
sudo service nginx configtest && sudo service nginx restart
</code></pre>

**TIP:** You can pinpoint problems in your nginx config using the following command

<pre class="command-line"><code class="language-bash">
sudo nginx -t
</code></pre>

* Create the neccessary `A` (IPV4) and `AAA` (IPV6) records, to point the required domain and the public IP.

## Install/Setup Certbot

**Note:** This step is optional, and is only required if you want to use SSL/HTTPS

* Ensure that your version of snapd is up to date

<pre class="command-line"><code class="language-bash">
sudo snap install core; sudo snap refresh core
</code></pre>

* Install Certbot

<pre class="command-line"><code class="language-bash">
sudo snap install --classic certbot
</code></pre>

* Prepare the Certbot command

<pre class="command-line"><code class="language-bash">
sudo ln -s /snap/bin/certbot /usr/bin/certbot
</code></pre>

* Get a certificate and have Certbot edit your Nginx configuration automatically to serve it, turning on HTTPS access in a single step

<pre class="command-line"><code class="language-bash">
sudo certbot --nginx
</code></pre>

* Test automatic renewal

<pre class="command-line"><code class="language-bash">
sudo certbot renew --dry-run
</code></pre>

* Confirm it worked by visiting your site over HTTPS


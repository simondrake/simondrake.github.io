---
layout: post
title: Create a self-signed certificate with OpenSSL
date: 2021-04-16
tags: []
---

Create a self signed certificate, with OpenSSL, by running the following command.

{:.line-numbers}
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365
```

You can also add `-nodes` (short for `no DES`) if you don't want to protect your private key with a passphrase. Otherwise it will prompt you for "at least a 4 character" password.

The `days` parameter (365) can be replaced with any number to affect the expiration date. It will then prompt you for things like "Country Name", but you can just hit Enter and accept the defaults.

Add `-subj '/CN=localhost'` to suppress questions about the contents of the certificate (replace `localhost` with your desired domain).

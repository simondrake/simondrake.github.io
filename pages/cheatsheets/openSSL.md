---
layout: page
title: OpenSSL
subtitle: OpenSSL is a software library for applications that secure communications over computer networks against eavesdropping or need to identify the party at the other end
permalink: /cheatsheets/openssl/
---

## Generation

Generate a new private key and Certificate Signing Request

<pre class="command-line"><code class="language-bash">
openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout privateKey.key
</code></pre>

Generate a self-signed certificate

<pre class="command-line"><code class="language-bash">
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out certificate.crt
</code></pre>

Generate a certificate signing request (CSR) for an existing private key

<pre class="command-line"><code class="language-bash">
openssl req -out CSR.csr -key privateKey.key -new
</code></pre>

Generate a certificate signing request based on an existing certificate

<pre class="command-line"><code class="language-bash">
openssl x509 -x509toreq -in certificate.crt -out CSR.csr -signkey privateKey.key
</code></pre>

Remove a passphrase from a private key

<pre class="command-line"><code class="language-bash">
openssl rsa -in privateKey.pem -out newPrivateKey.pem
</code></pre>

## Validating

Check a Certificate Signing Request (CSR)

<pre class="command-line"><code class="language-bash">
openssl req -text -noout -verify -in CSR.csr
</code></pre>

Check a private key

<pre class="command-line"><code class="language-bash">
openssl rsa -in privateKey.key -check
</code></pre>

Check a certificate

<pre class="command-line"><code class="language-bash">
openssl x509 -in certificate.crt -text -noout
</code></pre>

Check a PKCS#12 file (.pfx or .p12)

<pre class="command-line"><code class="language-bash">
openssl pkcs12 -info -in keyStore.p12
</code></pre>

## Debugging

Check an MD5 hash of the public key to ensure that it matches with what is in a CSR or private key

<pre class="command-line"><code class="language-bash">
openssl x509 -noout -modulus -in certificate.crt | openssl md5
openssl rsa -noout -modulus -in privateKey.key | openssl md5
openssl req -noout -modulus -in CSR.csr | openssl md5
</code></pre>

Check an SSL connection. All the certificates (including Intermediates) should be displayed

<pre class="command-line"><code class="language-bash">
openssl s_client -connect www.paypal.com:443
</code></pre>

## Converting

Convert a DER file (.crt .cer .der) to PEM

<pre class="command-line"><code class="language-bash">
openssl x509 -inform der -in certificate.cer -out certificate.pem
</code></pre>

Convert a PEM file to DER

<pre class="command-line"><code class="language-bash">
openssl x509 -outform der -in certificate.pem -out certificate.der
</code></pre>

Convert a PKCS#12 file (.pfx .p12) containing a private key and certificates to PEM

<pre class="command-line"><code class="language-bash">
openssl pkcs12 -in keyStore.pfx -out keyStore.pem -nodes
</code></pre>

You can add -nocerts to only output the private key or add -nokeys to only output the certificates.

Convert a PEM certificate file and a private key to PKCS#12 (.pfx .p12)

<pre class="command-line"><code class="language-bash">
openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile CACert.crt
</code></pre>


[ref](https://www.sslshopper.com/article-most-common-openssl-commands.html)

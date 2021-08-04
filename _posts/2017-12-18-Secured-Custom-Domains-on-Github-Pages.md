---
title: Secured HTTPS Custom Domains on GitHub Pages
tags: [IT, HTTPS]
redirect_from:
    - /blog/https-for-custom-domain-on-github-pages/
---

Needless to say, GitHub Pages is now one of the best and most convenient services to host static webpages. It's free, easy to manage (via `git` version control) and able to process markdown files directly.

However, it's not always good-looking and easy to remember to use a `USERNAME.github.io` as domain name of a personal website. It's simple to set up a custom domain name - and I'll introduce how to do it first.

## Set custom domain name

In case of GitHub Pages for a specific account,

1. Ensure you've created a repo named `USERNAME.github.io` (USERNAME is your account username)

2. Click `Settings` at the top right in the repo.
![Pic2](/assets/images/legacy/5a37752ff0093.png)

3. Find Custom Domain in GitHub Pages.
![Pic3](/assets/images/legacy/5a3780cd16ad9.png)

4. Fill in the domain name you purchased on any registrar (e.g. GoDaddy, Namecheap, Google Domains, etc.)

5. Go to the dashboard of your registrar and set `A Record`, `CNAME Record` to point or redirect to `USERNAME.github.io`.

In step 5, do know about different types of records before configuration. If either `ALIAS`, `ANAME` or CNAME flattening[^1] is supported by DNS service provider, it could be easier. In my case, Cloudflare supports CNAME flattening, so I set a CNAME record for my apex domain name `maristie.com`.

Refer to https://help.github.com/articles/setting-up-an-apex-domain-and-www-subdomain/ for further information.

## HTTPS support

Now it should be easy to access your personal website with your own domain name. But GitHub Pages does **NOT** support SSL/TLS at present.

It's easy to understand why GitHub has not provided TLS yet. In cryptography, we ensure that communications between two sides are safe with asymmetric encryption nowadays, using algorithms like RSA, ECC and ElGamal. Private key should **NEVER** be transferred over Internet, and thus it's just extremely dangerous to upload private key to GitHub. The only way to achieve it is to issue certificates and preserve them on GitHub servers, just like what Cloudflare does.

Anyhow if we would like to access personal websites with HTTPS, it does cost several more hours. Based on some other tutorials listed in references,[^2][^3][^4][^5][^6] here is a simple guide.

### Create an account for Cloudflare

Cloudflare mainly provides services like DNS, CDN and Web security. Here we concentrate on TLS configuration.

### Add site to Cloudflare

Follow instructions given by Cloudflare, and finally change nameservers from original registrar to Cloudflare.

### Configure SSL/TLS

Now it comes to the crucialc step. Click Crypto and check if SSL level is `Full` (neither `Flexible` nor `Full (strict)`). Some guides would suggest to select `Flexible`, which is out of date. In `Flexible` mode, there would be no SSL connection between Cloudflare and GitHub Pages servers, and it would cause security problems.

![Pic](/assets/images/legacy/5a379c66c0b92.png)

A simple picture shows the differences between the modes.

![Differences](https://blog.cloudflare.com/content/images/2016/06/cloudflare_ssl_modes.png)

### Enforce HTTPS

Since HTTPS provides secure connections between clients and servers, it's preferred that any request from clients be encrypted. It's not necessary to use Page Rules to enforce HTTPS. Cloudflare has provided a one-lick button in Crypto.

![Pic](/assets/images/legacy/5a379f5c4cf26.png)

## Extra optimizations

Now the personal website is under the protection of TLS. In addition, there're some more things Cloudflare can do for us.

### DNSSEC

> DNSSEC protects against forged DNS answers. DNSSEC protected zones are cryptographically signed to ensure the DNS records received are identical to the DNS records published by the domain owner.

Note that some domain extensions do not support DNSSEC like .site.

### HSTS, Modern TLS and automatic HTTPS rewrite

As their names imply, they provide advanced security configuration for users.

### BETA functions

There're several experimental settings in Speed section, such as Auto Minify and Rocket Loader. It's not recommended to turn them on as static webpages in blogs are generally light-weighted, hence it won't take much effect.

## References

[^1]: https://support.cloudflare.com/hc/en-us/articles/200169056-CNAME-Flattening-RFC-compliant-support-for-CNAME-at-the-root

[^2]: https://gist.github.com/cvan/8630f847f579f90e0c014dc5199c337b

[^3]: http://www.curtismlarson.com/blog/2015/04/12/github-pages-google-domains/

[^4]: https://hackernoon.com/set-up-ssl-on-github-pages-with-custom-domains-for-free-a576bdf51bc

[^5]: https://support.cloudflare.com/hc/en-us/articles/218411427#https

[^6]: https://blog.cloudflare.com/secure-and-fast-github-pages-with-cloudflare/

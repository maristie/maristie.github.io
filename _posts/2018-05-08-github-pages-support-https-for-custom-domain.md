---
title: "Good news: GitHub Pages support HTTPS for custom domains"
---

_**Note**: as of May 9, 2018 GitHub Pages only issue certificates of the only domain filled in `Custom Domain`, therefore it would be impossible to access this website with `https://www.maristie.com` since the certicate does not include www subdomain._

Late-coming feature for GitHub Pages: [Custom domains on GitHub Pages gain support for HTTPS ](https://blog.github.com/2018-05-01-github-pages-custom-domains-https).

GitHub partnered with the famous free CA *Let's Encrypt*, and SSL/TLS certificates are now available directly on GitHub Pages without any intermediary like Cloudflare.

## How to

For ones using `ANAME`, `ALIAS` or `CNAME` records to resolve custom domains, the only thing to do is check the option box `Enforce HTTPS`. For others using `A` records, first update the IPv4 addresses of the records to

- 185.199.108.153
- 185.199.109.153
- 185.199.110.153
- 185.199.111.153

After the update of records comes into effect, complete configuration by checking the same box.

## Why

Before that lots of us (including myself) are using Cloudflare as a suboptimal choice to implement SSL/TLS connection. However, even if we select the `Full SSL (Strict)` mode, Cloudflare still works as an intermediary in the communication between client and server, which means our data is decrypted at Cloudflare. It is not an end-to-end encryption.

![Full SSL (Strict) Mode](https://support.cloudflare.com/hc/en-us/article_attachments/206167947/cfssl_strict.png)

By directly using the certificate signed by Let's Encrypt and stored on GitHub servers, a true end-to-end secure connection is established between clients and our websites.

The drawback is also obvious: the private key is under the control of GitHub, not ourselves. Thus the communication is based on our trust in GitHub, and we host our private keys on GitHub servers. It is advised that,

> GitHub Pages sites shouldn't be used for sensitive transactions like sending passwords or credit card numbers.

## Additional tip

Do not forget the `www` subdomain. In [GitHub Help about www subdomain](https://help.github.com/articles/setting-up-an-apex-domain-and-www-subdomain/),

> If your domain has HTTPS enforcement enabled, GitHub Pages' servers will not automatically route redirects. You must configure www subdomain and root domain redirects with your domain registrar.

Therefore it would be better to configure a redirection or `CNAME` record from `www` subdomain or apex domain to `USERNAME.github.io`.

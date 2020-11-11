---
title: Prepare Website for HTTPS
tags: [IT, HTTPS]
redirect_from:
    - /blog/prepare-for-https/
---

[Let's Encrypt](https://letsencrypt.org/) is a famous free SSL certificate provider. Now take this as an example to build up HTTPS on your website.

First you should download a script.

```bash
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
```

If you're using Apache, then

```bash
sudo ./path/to/certbot-auto --apache
```

Then the script will do most of the things for you and just wait.

One problem that you might encounter is that the script could prompt an error that it can't find some package. Don't worry. First ensure that you've install python and pip on your server, and then upgrade pip.

```bash
/root/.local/share/letsencrypt/bin/pip install --upgrade pip
```

After that you just need to install the lacking packages with `pip install` one by one. It's very fast.

Finally, we should notice that the certificate is only valid for 3 months.

> Certbot can be configured to renew your certificates automatically before they expire. Since Let's Encrypt certificates last for 90 days, it's highly advisable to take advantage of this feature.

It's better to arrange for automatic renewal by `cron` or `systemd`. For example,

```bash
crontab -e	# edit crontab file
0 1,13 * * * /home/ec2-user/certbot-auto renew
```

It's recommended to run twice a day. The above command means renew certificate at 1:00 and 13:00 every day.

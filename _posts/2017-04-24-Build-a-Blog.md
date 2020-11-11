---
title: Build a Blog
tags: [IT]
redirect_from:
    - /blog/build-blog/
---

## To start with

Well I'd never written something like a blog before... So this is the first time I tried with Hugo to create my own blog. At first I've never thought of getting a blog for I was just trying to build a proxy server for my own use. But I thought that was just a waste of the server and thus I turned to make a blog.

In fact I'm quite grateful to my previous teacher who had given a lesson named *Web Software Technology* (I hadn't got a very satifactory score though), which I think should be called *Web Software Archaeology*. I've done badly in the final exam and was not in a good mood for quite a long time - but now I'm feeling a bit grateful since at least I've learned how to make use of **[Apache](https://www.apache.org/)** and **[PHP](https://www.php.net/)**.

## Would you like a blog of your own?

To be honest this can't be called a "difficult procedure"... To set up a web server it's much easier than most people think - even for those who are just new to programming. On your virtual private cloud, just log in and execute `sudo yum install apache` and that's all. You needn't even know how to compile the source code (commands like `make` or 'make install').

Something that might be a little difficult is that you should find some blog framework that is suitable for you like **[Hugo](https://gohugo.io/)** which I'm using now. Some frameworks even help you build a server and you just need to configure the listening port and then you can begin to write an article. This one can do things like that but I prefer to build a server myself - in that way I can clearly understand what the server is doing in log files. Therefore I just use it as something to help me transform markdown to html and render it with a beautiful style.

As what I've said above Hugo can be made use of even on your local computer. You can maintain a directory to store your markdown files and transform them to htmls just on your own computer. The only thing you should do after writing is just upload them to virtual cloud - just like me. To install hugo, that's also very easy - `apt/yum install hugo` (`apt` for *Debian* and `yum` for *RedHat*) or something else if you're using *MacOS* or *ArchLinux*. Then just look up everything you need in hugo's official documentation.

As an example,

```bash
hugo help
hugo version
hugo new site [your site name]
hugo new post post/[your post name]
cd themes
git clone https://github.com/dim0627/hugo_theme_robust.git
cd ..
hugo server --theme=hugo_theme_robust --buildDrafts
hugo --theme=hugo_theme_robust
```

But make sure you've installed git package.

## Final steps for your blog

Finally to upload files to your VPC, if taking *Amazon EC2* as an example, just execute the following in terminal:

```bash
scp -i /path/to/privatekey -r /path/to/src ec2-user@ec2-**-**-**-**.your.ami.dns:/path/to/dst
```
* privatekey - your private key to log to AMI
* src - your DIRECTORY which holds your htmls, for instance, /path/to/public for Hugo
* dst - your DIRECTORY which you'd like to put your htmls, for instance, /path/to/htdocs for Apache

And -i means you use an identification by pem (Privacy Enhanced Mail), -r means recursively transfer your files. Note that the asterisks refer to **[Dot Demical Notation](https://en.wikipedia.org/wiki/Dot-decimal_notation)** of your IP address. Of course IPv6 addresses are also supported by scp.

Now the last thing you should do is just configure secure group in AWS console. Allow visit from 80 port (which is http) by TCP protocol. That's all.

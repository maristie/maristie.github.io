---
title: Using Docker for Steam Card Farming
tags: [IT, Virtualization]
redirect_from:
    - /blog/docker-for-steam-card-farming/
---

Steam is a famous platform providing tens of thousands of PC games. Some of the Steam players prefer to wait for cards to drop naturally along with the progress of games. However, others may think it better to speed up the process of card collecting, either for themselves or even make a profit. Anyhow, in the information age, people would like to automate everything with the power of computers.

Here I'll make a simplified introduction of making use of [Docker](https://www.docker.com/) to farm cards, based on [ArchiSteamFarm](https://github.com/JustArchi/ArchiSteamFarm) on GitHub.

## Motivation

Perhaps many Steam players have heard and used ArchiSteamFarm (in short ASF). It's a piece of software written in C# (a programming language developed by Microsoft), which is good news for Windows users, but in turn bad news for macOS and Linux users.

It's not a rare problem in software development. In fact, some of C programs written on Linux cannot be transplanted to Windows, since dependencies may vary from OS to OS, and APIs are not consistent. It's a common demand to get software run on different platforms easily.

Let's think of VM (Virtual Machine). It's alright to run VMs on a server and run different software on different VMs, just like EC2 service provided by Amazon. The drawback is obvious that hypervisor-level virtualization (such as KVM or Xen) which runs different kernels is too coarse-grained and inefficient in scheduling for applications. Here comes Docker, a provider of OS-independent containers to run diverse software on the same kernel.

> Docker is a software technology providing containers, promoted by the company Docker, Inc. Docker provides an additional layer of abstraction and automation of operating-system-level virtualization on Windows and Linux.[^docker_wiki]

![Docker Image](https://www.docker.com/sites/default/files/d8/2018-11/docker-containerized-and-vm-transparent-bg.png)
_From https://www.docker.com/resources/what-container_

Take ASF for an example. For Windows users it's easy to configure everything since .NET core has been built-in. But for others, especially for Amazon Linux users, it's only a disaster of dependency. Imagine that you have to compile tens of dependencies and .NET core on VPS and overcome hundreds of bumps during the process.

## Instruction

Thanks to ASF for providing a Docker repo, which saves me several hours (not days I hope) to deploy .NET runtime environment. Now let's get started. It's just a simple version for ones who feel 'TL;DR' of the [official documentation]().

### Log in to your VPS and install Docker

No matter what OS (or distros of Linux) your VPS is using, you should be able to find Docker in the official repo with package management tools, like `apt` for Ubuntu and Debian, `yum` for CentOS and Red Hat, and `brew` for macOS.

### Pull repo from ASF repo hosted on Docker Hub

Here you may use different configurations for different purposes. I'm just giving a simplified and common instruction for personal users holding a VPS.

Before anything, in terms of security do **NOT** run Docker with `root` user without special cases. So after installation, add your present user to `docker` user group.[^docker_aws]

```sh
$ sudo usermod -a -G docker [YOUR CURRENT USERNAME]
```

Then log out and log in again to check if you're able to run Docker as a normal user by `docker info`.

Now it's OK to pull ASF of the `latest` tag (you may select other tags) to VPS:[^dh_asf]

```sh
# 'latest' tag by default
$ docker pull justarchi/archisteamfarm
```

### Configurations

Details about configurations are hosted on [GitHub Wiki](https://github.com/JustArchi/ArchiSteamFarm/wiki/Configuration). Without needs to trade, it's simple to edit the default `json` files, including `ASF.json` and `[YOUR BOT NAME].json`.

`ASF.json` is a global configuration file and mainly takes control of the entire runtime environment. Take care of only one entry:

```json
"Headless": false
```

`Headless` means that ASF should not ask for any information from interaction with user, as in a long term we're not always using `ssh` to keep up the teletype with VPS. At first we make it `false` to input the 2FA code on your mobile, but change it to `true` after the initial login.

Next, in `[YOUR BOT NAME].json` (`YOUR BOT NAME` is anything you'd like to name your card farming bot),[^asf_wiki]

```json
{
    "AutoDiscoveryQueue": true,
    "Enabled": true,
    "HoursUntilCardDrops": 1,
    "FarmOffline": true,
    "SteamLogin": "[YOUR STEAM USERNAME]",
    "SteamPassword": "[YOUR STEAM PASSWORD]"
}
```

- `AutoDiscoveryQueue`: you'd probably like to discovery the queue and get three free cards every day during summer or winter sale.

- `Enabled`: automatically run the bot when starting up ASF

- `HoursUntilCardDrops`: unknown parameter that could influence the performance of [card farming algorithm](https://github.com/JustArchi/ArchiSteamFarm/wiki/Performance)

- `FarmOffline`: farm cards with offline status

- `SteamLogin` & `SteamPassword` : literally, if you'd like to type manually every time ASF starts, just leave it blank

### Final steps

Keep the `json` files above in a directory (e.g. `/home/ec2-user/asf/config/`). Docker should share this directory with operating system (as Docker is a sandbox).

```sh
$ docker run -it -v /home/ec2-user/asf/config:/app/config --name asf justarchi/archisteamfarm
```

Done. Manage the container with

```sh
$ docker start asf
$ docker stop asf
```

Enjoy card farming!

## References

[^docker_wiki]: https://en.wikipedia.org/wiki/Docker_(software)
[^asf_wiki]: https://github.com/JustArchi/ArchiSteamFarm/wiki/
[^docker_aws]: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html
[^dh_asf]: https://hub.docker.com/r/justarchi/archisteamfarm

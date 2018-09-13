---
title: Intro to GnuPG
---

PGP (Pretty Good Privacy) is an encryption program that provides cryptographic privacy and authentication for data communication (Refer to: https://en.wikipedia.org/wiki/Pretty_Good_Privacy). And OpenPGP is a standard that PGP software should follow. One of the software is [GnuPG](https://www.gnupg.org/).

GnuPG helps people to protect their privacy by encrypting or signing their mails and guarantees that only the recipient could decrypt them or others could validate the content. It's not under any government regulation, which in a sense means 'absolute' freedom.

In many distros GnuPG has been built in. If not, find it in official repos.

```shell
$ gpg --generate-key  # generate a new pair of keys
```

It's quite simple to get your own private key and public key. The former one is for decryption, and latter one for encryption (though this statement is not so rigorous).

Then you can send your public key (including subkeys) to a key server like hkp://keys.gnupg.net (`hkp` is a protocol specialized for PGP).

```shell
$ gpg --keyserver keys.gnupg.net --send-key [last 8 chars of your key fingerprint]
```

Now you're able to encrypt your e-mails with GnuPG in E-mail management software like Evolution. If you would like send others an e-mail, use his public key. But do remember that it's better to check the fingerprint of the key via telephone, messaging software or ask him in person.

Here be careful that the master public key is usually used for certify your subkeys. It's recommended to use subkeys for encryption or signing, since it'll be much easier to revoke a subkey rather than a master key.

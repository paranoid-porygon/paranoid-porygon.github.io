---
layout: post
title:  "MFA all the things: I got a yubikey and you should, too!"
summary: "How to set up a yubikey in Linux for use with KeePassXC"
date:   2024-09-29 15:15:03 -0400
last_updated:
tags: [howto, security]
published: false
assetdir: 
---

## Which Keys

First of all: **get at least two keys.** God forbid you set up just one key for all your mission critical applications only to see it get lost, stolen, or damaged and get permanently locked out of all your accounts. Bad time. Very bady time. 

I went with one [Yubikey 5 Nano][yk5n] as my main key because I wanted something low profile, and a [YubiKey 5 NFC][yk5nfc] as my backup to keep in cold storage because it was the cheapest of their current-generation keys.

## Setup

My primary use for these keys is to act as a challenge-response token for my KeePass password database. I know that KeePass offers for you to generate a keyfile as a "something you have" factor of secondary authentication, but my thinking is that if an adversary has access to your machine such that they can copy your password database file, they can likely also copy your keyfile. What they can't copy, however, is your Yubikey, so it seems to me as a better method of "something you have" secondary authentication.

### Software Installation

Following the [Arch Wiki guide][aw-yubikey] for setting up a yubikey, I took the lazy approach and installed the GUI [yubico-authenticator][aur-yubico-authenticator] to provision the keys.

### Provisioning the keys

Plug in both keys at the same time.

Configure the slot for short touch on the primary key to use *challenge-response*, generating a new random key and **making sure to copy it to your clipboard**, and making sure to select "require touch".

Then switch to the backup key, configure the slot for short touch to use *challenge-response*, paste the key you had previously generated **otherwise this will not work as a backup**, and selected "require touch". Botch keys should now work for the same challenge-reponse action. Make sure to clear your clipboard.

[aw-yubikey]: https://wiki.archlinux.org/title/YubiKey
[aur-yubico-authenticator]: https://aur.archlinux.org/packages/yubico-authenticator
[pg]: https://www.privacyguides.org/articles/2025/03/18/installing-keepassxc-and-yubikey/#step-4-prepare-your-yubikeys

---
layout: post
title:  "Homelab: Quickly setting up SyncThing as a Dropbox replacement"
summary: "Very quick and easy SyncThing setup if you already have a TrueNAS [Scale] machine up and running"
date: 2025-12-01  
tags: [homelab, dealgorithming]
published: true
---

I have been planning to ditch Dropbox, Google Drive/Backup, and MEGA in favor of [SyncThing][syncthing] for several months but kept getting sidetracked. At the end of my Thanksgiving break last night and due to my now having 3 different laptops and 3 different desktop OS instances that need to share KeePass password files and [vimwiki][vimwiki] markdown files, I finally sat down and deployed SyncThing on my [TrueNAS Scale][truenas] VM. Thanks to the below [YouTube video](https://youtu.be/ITro3Bws7JQ), it was extremely simple.

**VERY BIG WARNING NOTE:** Do **not** allow SyncThing to run as root as he recommends in this video. It is neither necessary nor safe. Leave its user id as the defaul app user id.

## Client installation on Arch linux

The video goes over how to setup a client (specifically on Windows) after it had already been installed. That setup process applies to other operating systems so I won't reiterate it, but I will show you how simple it is to install SyncThing on Arch linux. You'll want to do this before following the video's instructions.

Drop into a shell logged in as a sudoer (not root) and run the following:

```
sudo pacman -S syncthing
systemctl --user enable --now syncthing
```

This will install SyncThing and then enable the SyncThing systemd service to run once the user who enabled the service logs in (so not at boot).

After that point, continue following the video tutorial for client setup.

## "Server" setup on TrueNAS Scale

<iframe width="100%" height="315" src="https://www.youtube-nocookie.com/embed/ITro3Bws7JQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

[syncthing]: https://syncthing.net/
[keepass]: https://keepassxc.org/
[vimwiki]: https://vimwiki.github.io/
[truenas]: https://www.truenas.com/

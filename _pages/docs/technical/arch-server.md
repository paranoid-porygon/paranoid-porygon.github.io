---
layout: default
title: "HOWTO: setting up a server using Arch Linux"
permalink: /docs/arch-server
---

# Overview

I recently set up a Proxmox server and have been deploying a variety of VMs on it: a NAS, a seedbox, a Windows 11 desktop if I absolutely need to use Windows, a Kali instance for CTFs, an OSINT VM that I can quickly duplicate for different investigations. But increasingly I have been needing/wanting to set up small, single purpose VMs without having to repeat this install process every time. Proxmox luckily offers the option to convert existing VMs to ["templates"][proxmox-vm-templates] to use in quickly deploying new machines based on the template. Below I explain how I set up my base server template using Arch linux.

Quick note: I understand that Docker and LXC containers are a thing. I choose not to use them because I do not want the resources of my Proxmox server being shared without the level of abstraction provided by a true VM. Additionally, I have more experience with hypervisors than I have with container deployment software so doing it this way is the path of least resistance. 

Additional note: I also understand that Arch is not considered a stable distro suitable for servers and that Debian, Fedora, or Ubuntu might have been preferable. There is plenty of documentation on the internet about how to setup a server on those distros, but I personally use Arch on my desktop/laptops so I again opted for what I was most familiar with. Were this an internet-facing server that more people relied upon than just my household, I would instead use Fedora.

# Installation

When selecting a kernel, you need to pick `linux-lts`. This is a server so we want maximum stability, inasmuch as possible for a rolling release distro like Arch, which forces us to pick the long-term service version of the Linux kernel.

Don't create any users yet. We will reserve that for once the server is spun up with a purpose in mind and users and permissions will be created and delegated on an as-needed basis.

# Post-install setup

```
sudo pacman -S apparmor firejail man-db iptables rkhunter clamav inetutils s-nail cronie less vim base-devel git wget ssh
```

## Apparmor

To enable mandatory access control, we need to enable apparmor.

## Firejail

Sandboxing is generally a good practice and this is especially true for servers.

You will need to create ad hoc firejail profiles once you set up the server for a specific purpose.

## Firewall

## Scheduled antivirus and rootkit detection

## Prepare ssh access for nonroot users

# Deployment

## Change hostname

## Add users

### Enable ssh

[proxmox-vm-templates]: 



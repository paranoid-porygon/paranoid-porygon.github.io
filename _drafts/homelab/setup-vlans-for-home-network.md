---
layout: post
title:  "Setting up VLANs on my home network"
summary: "Finagling OPNsense, Proxmox, and various MikroTik switches to play nicely together"
date:   
last_updated:
tags: [homelab]
published: false
assetdir: 
---

I recently, finally got around to setting up a DMZ on my home network and am also setting up a variety of VLANs for different use cases (IOT, a gaming VLAN for consoles and for guests to plug into for LAN parties, a management VLAN, and a VLAN for various self-hosted services).

## Basic VLAN setup with OPNsense, Proxmox, and whatever switch you are using

My network stack isn't anything unique: an OPNsense firewall/router, a couple of MikroTik switches and wireless access point, and a Proxmox host with a variety of VMs running on it. The below video coverd the OPNsense and Proxmox portion of that stack/

{% include youtube.html vid="Dw8OYJtexUE" %}

I'll hit the important bullet points if you don't want to watch a whole video.

### OPNsense setup

#### Create VLAN

In the side menu, go to **Interfaces -> Devices -> VLAN**.

Click the orange **+** sign in the lower right corner of the panel.

In the panel that pops up, fill out the fields thusly:

* **Device**: leave blank
* **Parent**: your lan interface
* **VLAN tag**: pick a number that isn't `1`; you ideally want this to match the subnet ID you will use for this VLAN
* **VLAN priority**: if you know you need something besides `Best Effort (0, default)`, pick that
* **Description**: a verbose description of what you're going to use this VLAN for (e.g. IOT, DMZ)

Hit the orange **Save** button. Then hit the orange **Apply** button after the panel disappears. You'll see your new VLAN pop up with an auto-generated name. I suggest leaving it named something like `vlan000` but if the number doesn't match the VLAN tag you picked, feel free to edit the entry, save, and apply.

### Switch setup

### Proxmox setup

I however had to find other documentation to get the VLANs set up properly on my MikroTik switches.

## Inter-VLAN networking



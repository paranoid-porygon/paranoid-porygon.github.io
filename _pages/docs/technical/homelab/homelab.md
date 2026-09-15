---
layout: default
title: "Homelab Overview and Documentation"
permalink: /docs/homelab
---

# Homelab

This page and its children are documentation covering how I set up my own homelab, home network, various servers and machines on that network, and related procedures. This is here mostly for my own reference in case I can't access my local documentation or suffer a catastrophic outage (i.e. my house burns down) and need to rebuild from scratch, but it should also be useful to anyone interested in building their own home lab/network and want to copy my homework.

## Machines

* Network stack (aka top of rack)
    - [OPNsense][opnsense] (router/firewall)
    - Mikrotik 2.5Gbe managed switch
    - Mikrotik 1Gbe PoE switch
    - Mikrotik wAP
* Proxmox (hypervisor):
    - TrueNAS Scale VM (Storage and related apps/services)
    - seedbox (yar har)
    - web server
* AI server
* Power stack:
    - Automated Transfer Switch
    - UPS and battery expansion bays

### To-implement

* ethernet power controller / smart power strip
* Network Video Recorder
* Hardware NAS

## Services

* Syncthing
* LinkWarden
* Immich
* [TODO] \*arr stack
* [TODO] Jellyfin
* [TODO] ersatzTV
* [TODO] NVR




[opnsense]: /docs/homelab/network/opnsense

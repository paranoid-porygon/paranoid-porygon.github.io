---
layout: post
title:  "Homelab - Part 00: Why, Current Hardware, and Planned Software"
summary: "Introduction to my homelab project"
date: 2025-08-21  
tags: [projects, homelab]
published: true
---

In 2020 shortly after the US entered a quasi-lockdown state due to COVID-19, my employer laid off my entire team for spurious reasons. My boss, also laid off, had decided by that point (probably due to America's cultural aversion to taking COVID seriously) to move back to Canada. During the process of packing for his move, he bequeathed me an old Dell Poweredge T420 that he had been loaning to our former employer up until his termination. With 16 2.5" drive bays, I knew immediately that this would become a Network Attached Storage (NAS) server for my home; having messed around with FreeNAS back in 2015, I had some familiarity with what a self-hosted storage/media server had to offer and missed having one. However, the ongoing pandemic lockdown, tumultuous job hunt, and need to move to a new home multiple times over the subsequent four years saw me postpone that project.

Now that I'm in a stable housing and employment situation, and given the rapid enshittification of cloud services over the last five years (and especially the capituation of certain big tech companies to the whims of our nascent authoritarian government), the time has come to not just spin up a NAS on this old Poweredge but to build out an entire homelab around it with the express goal of divesting from as many non-selfhosted services for which I can find open source/libre self-hosted alternatives.

<!--excerpt-->

# Homelab Goals

The general idea behind homelabbing is collecting various servers, networking devices, and other peripherals to serve as a playground meant for experimentation and self-education for various IT concepts and practices. Most people who homelab opt to purchase used/retired equipment that previously belonged to small businesses both to save money and to minimize what would otherwise become e-waste.

In building out *my* homelab, these were the software and services that I planned to host and needed appropriate hardware to accomodate:
* a NAS
* a Google Drive/Dropbox replacement
* torrenting software and associated automation utilities
* various media servers
* IP cameras to replace my Nest camears
* a replacement for the Google Home smart home automation ecosystem
* a general purpose VM host to spin up ad hoc virtual devices, microservices, Docker containers, etc.
* a customized DNS
* a custom router/firewall appliance
* resleeving my gaming computer as a rack-mounted machine
* any necessary network devices to support the above
* an Uninterruptible Power Supply (UPS) that could provide normalized power and battery backup to the above
* a transfer switch to allow for switching power to the devices between the UPS and the wall outlet during mbattery maintenance

# Prior Attempts

My previous experiments with the now-defunct FreeNAS on an old desktop PC saw it being used as a seedbox (a dedicated bittorrent machine). It worked well, however there were some issues that I knew would become future problems: the usage of non-ECC RAM, the operating system being installed on a single USB stick as opposed to redundant drives, the usage of only two HDDs in a striped configuration for storage. Real amateur hour stuff.

I also had a PFsense router that I had put together as cheaply as possible (to the point that I didn't even buy a case for it). It didn't work as well as I had wanted -- it experienced significant and frequent bandwidth issues -- but it allowed me finer control over network traffic, which was useful at the time because I had exposed a SQL database to the internet for my coworkers to access.

Both of these were completely abandoned and redone from the beginning.

# Current Setup

After a couple of months of preliminary research and knowing that I planned to mount all these devices on a 19" wide standard server rack (dictated by the Poweredge I was building around), I estimated that I would need a four-post server rack with at least 25 rack units of vertical mountable space. I was very fortunate to find someone within an hour of where I lived selling a Startech rack at exactly 25U tall on Facebook Marketplace at an extremely reasonable price and in exceptional condition, so I jumpted on it and picked it up ASAP.

## Layout

Rack acquired, I sketched up a preliminary layout of both the front and back of the rack with the devices I planned to use. As I purchased needed devices and refined my needs, I landed on the below layout:

+: already acquired and installed
-: possible future purchases

| 25U Planned Config                  | row | Back                                   |
|-------------------------------------|-----|----------------------------------------|
| + 1U: Firewall (OPNsense)           | 01  | + 2U: Shelf for modem, hue bridge, WAP |
| + 1U: MikroTik 310 <> MikroTik 112  | 02  | + ^^                                   |
| - 1U: brush/patch panel             | 03  | + 1U: StarTech cable bracket           |
| - 1U: SFP+ switch                   | 04  |                                        |
| - 1U: IP Camera Server, aka NVR     | 05  |                                        |
| - 2U: JBOD / NAS                    | 06  |                                        |
| - ^^                                | 07  | - 1U: Power Strip                      |
| + 5U: Gaming PC (Bazzite)           | 08  | + 5U: Gaming PC                        |
| + ^^                                | 09  | + ^^                                   |
| + ^^^                               | 10  | + ^^^                                  |
| + ^^^^                              | 11  | + ^^^^                                 |
| + ^^^^^                             | 12  | + ^^^^^                                |
| + 5U: Dell Poweredge T420 (Proxmox) | 13  | + 5U: Dell Poweredge T420              |
| + ^^                                | 14  | + ^^                                   |
| + ^^^                               | 15  | + ^^^                                  |
| + ^^^^                              | 16  | + ^^^^                                 |
| + ^^^^^                             | 17  | + ^^^^^                                |
|                                     | 18  |                                        |
| + 1U: APC Transfer Switch           | 19  |                                        |
| + 2U: APC 1500VA UPS                | 20  | + 2U: APC 1500VA UPS                   |
| + ^^                                | 21  | + ^^                                   |
| + 2U: Extended Battery              | 22  | + 2U: Extended Battery                 |
| + ^^                                | 23  | + ^^                                   |
| + 2U: Extended Battery              | 24  | + 2U: Extended Battery                 |
| + ^^                                | 25  | + ^^                                   |
| ----------------------------------  |     | -------------------------------------- |
|                                     |     |                                        |

## Hardware

From top to bottom, here's what I installed and why. I'm only going to do a cursory explanation of my software choices for the sake of this post; I will likely follow up with future blog posts outlining my rationale and specific configurations for each machine, VM, or service.

### OPNsense Firewall

To replace my PFsense firewall from 7 years ago, I completely rebuilt the machine (this time with a nifty SuperMicro case that rotates the motherboard IO ports to face frontwards!) using the OPNsense fork of PFsense. I don't have enough knowledge of firewalls and networking at this point to have any strong preference besides that OPNsense is a community open source fork after PFsense went proprietary. PFsense is supposedly better, but I'm not skilled enough to utilize the features that make it such and prefer to use software that doesn't require me to rely on the developer to remain benevolent when they could instead just simply open source their project in keeping with the zero trust security paradigm.

The hardware in the firewall is nothing special: older Intel dual core chip, 8Gb ram, mini-ITX motherboard. 

The only remotely interesting internal component is a quad-2.5Gbe NIC that I installed. OPNsense does not play nice with Realtek NIC chipsets so I went with an Intel I226-V ethernet controller. I tried my absolute damnedest to find an ethernet controller that isn't made in mainland China (using Chinese network devices, motherboards, and storage scares the absolute shit out of me), however I couldn't find anything that was made elsewhere (ideally Taiwan), so I settled for the one device I could find on Amazon but *not* on AliExpress. I'll just have to monitor network traffic to verify that the NIC isn't compromised in any way.

### Network Switches

Most folks recommending home networking devices to prosumers will usually advise to "just buy Ubiquiti/Unifi stuff and be done with it", citing its "single pane of glass" network management user interface, ease of installation and setup, and sexy brushed aluminum cases as selling points. The other options were used business-class network switches, which are noisy and power-hungry and frankly overkill for a home network, and the MikroTik brand of network devices, which were described to me as the Android to Ubiquiti's iPhone.

Since I am an obstinate piece of shit who never turns down an opportunity to do things the hardest way possible, I went all-in on the MikroTik ecosystem. In addition to granting more granular control over a network than Ubiquiti and using less power than a outdated Brocade or Cisco switch, MikroTik doesn't require users to link ther switches to a cloud service like Ubiquiti apparently does.

#### Mikrotik 310

This is an 8-port 2.5Gbe managed switch that includes 2 SFP+ ports. There are a couple of things I don't like about it, such as how its power supply plugs into the front as opposed to the rear and there's no means of redundant power like other MikroTik devices at a similar price point and form factor, however it is their most reasonably priced 2.5Gbe switch at the time of writing this. While my home internet is currently only 0.5Gbps down and up, I wanted to both futureproof myself in case I decide to upgrade and allow for fast wired LAN data transfers to and from the NAS.

#### MikroTik 112

This is an old generation managed switch; I saw a handful of Reddit users cautioning against buying any 100-series MikroTik devices since they have been phased out and are allegedly notorious for causing network issues. All that said, I ultimately purchased it because:
1) it was the lowest-priced MikroTik managed switch that I saw that supported PoE and 1Gbe speeds
2) it was the correct size to place next to my MikroTik 310 in order to completel fill a single rack unit on my server rack

#### MikroTik WAPx

I purchased a wireless access point from MikroTik in order to have a consisten user experience when managing my network devices. Unfortunately it wasn't until after I purchased it that I discovered that MikroTik has notoriously not-great wirless devices and that their wired switches are their bread-and-butter. Oh well.

### Gaming PC

With the impending sunset of Windows 10 in October and some extremely unsettling features and poor performance in Windows 11, and given the recent and enormous advances in gaming on Linux thanks to Valve throwing their weight behind it, I decided last year to make a hard pivot to Linux on all my personal devices, including my gaming PC. I settled on the Bazzite distro for this machine; a fork of Fedora, its aim is to provide a similar experience to playing on a Steam Deck but on the desktop. 

The pivot to Linux also demanded a slight rework of my gaming PC's internals as well. While Bazzite *can* support nVidia graphics cards, Linux has always played nicer with AMD GPUs, and my 1080Ti was finally starting to show its age when trying to play STALKER 2. Coincidentally, the three 1080p BenQ monitors I purchased 8 years ago had all begun to fail, and I knew when the time came I would replace them with a single 5120x1440p curved monitor. Given those criteria, I used the opportunity to upgrade to an RX 7900XTX since the 24Gb of VRAM would enable smoother rendering on the super-ultrawide monitor I planned to purchase.

The monitor I ultimately purchased was an LG 45" 5120x1440p curved monitor with a 200hrz refresh rate. I opted against getting an OLED since I knew that this would be used for gaming and I was terrified of "burn-in", especially on an expensive monitor that I expect to last ~10 years.

For years I had kept tabs on what people recommended as far as "rackmount gaming PC cases" and never been satisfied with their suggestions (usually cheap Rosewill cases that didn't properly fit AIO cooling units). I became aware of the Silverstone brand during a trip to Microcenter when I asked a salesperson what case would make for a good game console replacement and they showed me the Silverstone GD09; it was conspicuously server-sized, and I kept it in the back of my mind if I ever wanted to make a home theater PC (HTPC). Realizing that I would have almost enough components leftover after I ship-of-Theseus'd my gaming PC to finally build an HTPC, I checked out Silverstone's website to see if they still made the GD09. Not only did they, but they were already on to the GD11 which was specifically designed to support 240mm AIO liquid cooling units (which was exactly the type of cooler I had), *and* they sold rackmount ears that attach to all of their GDxx line cases! All that was needed was a shelf or set of generic rails that fit my rack and could support the weight of the PC (which I found for less than $40 on Amazon), and this was exactly what I wanted in a case.

While I technically had until October to get this PC put together (well, October 2028 if you want to pay for Microsoft's promised 3 additional years of ongoing paid Windows 10 support, but I'll be damned if I have to give Microsuck another cent), I went ahead and purchased all the new components by early August since it was looking likely that all the stock that retailers had purchased before Trump's tarriffs went into effect was about to dry up, and subsequently both prices and product availability were about to become fairly nasty. For what it's worth, I don't consider this a panic purchase; I had known since Microsoft announced the Windows 10 sunset date that I would make this purchase sometime before October of 2025, so it really only moved my timetable up by a month.

After putting this thing together and using it for around two weeks playing a variety of old and new single- and multi-player games designed for Windows, I have to say that my expectations were vastly exceeded. I could not believe how well games run on it, or how the Bazzite team packed everything you need, and nothing more, to play games on Linux. It is an utterly phenomenal experience, and since my gaming PC was the final Windows holdout, Bazzite allowed me to finally make the full pivot to Linux on all my bare metal machines.

### Dell Poweredge T420

The progenitor of this homelab project features two CPU sockets and support for up to 392Gb of RAM, two redundant power supplies, two ethernet controllers supporting 1Gbe speed, 16 hot swappable 2.5" drive bays attached to a RAID controller and two more internal SATA headers. All of that to me screamed "hypervisor". This thing was meant to host VMs.

I know it's in vogue for homelabbers to build a cluster of Raspberry PIs or Lenovo ThinkCentres to host their VMs but I decided against that for the following reasons:
1) I already owned the Poweredge
2) its beefy hardware allows me more headroom if I need to dial up how many cores or how much memory a VM needs
3) I have run into compatability problems with arm chipsets in the past and instead try to stick with Intel whenever possible
4) I am skeptical that it's easiser to manage a cluster over a single machine

I was able to purchase a sliding rail that allowed me to mount the machien to the server rack (it was cheaper on Amazon than on eBay, would you believe). My only regret was blindly picking a spot on the rack to start mounting things (this was the first addition) because now there's an awkward space between the Poweredge and the Automated Transfer Switch below it.

The Poweredge is currently running Proxmox, a hypervisor that is just meant as a tool for easily spinning up and managing virtual machines. So far, I am extremely impressed with it and wish I had tried it out years ago rather than running all the VMs I needed on my desktop inside VirtualBox.

Proxmox will act as my playground for different self-hosted services and servers. At the moment, the only VMs I have running on it are a TrueNAS (the successor to FreeNAS) Core image that acts as my NAS (and also does nested virtualization of some Docker containers for NAS-adjacent services like SyncThing), and also a Windows 11 VM just so that I have a working copy of Windows in case I absolutely need one. I plan to add the following services at some point in the future:
* HomeAssistant
* PiHole or another private DNS
* Plex or another media server
* a bittorrent client, torrent file finder, and automatic media organizer like I had set up many years ago
* a private LLM
* some sort of note taking / knowledgebase app to replace VimWiki that I currently use
* dedicated servers for games (Palworld, DayZ)
* a library app / Goodreads replacement to track my books and other physical media
* a private search engine
* a network video recorder (which will eventually be spun off to bare metal hardware of its own)
* an Immich instance
* an SMTP server (this is not a high priority; email is a very intensive service that requires constant uptime and trust of the other party)

As far as hardware upgrades, I've only upgraded the RAM at this point to 96Gb for the single 8-core processor that I have. Eventually, I will upgrade both CPU sockets to the maximum 10-core proc that is compatible with this socket, and give both processors 96Gb of RAM.

### Power Stack

I want to be clear: I went a little overboard here and I think I regret it. The devices currently on this rack have only ever used maybe 40% of my UPS's total output capacity, so perhaps the two external battery expansions were overkill. But if a prolonged power outage occurs and I need to plug in, say, my CPAP machine overnight, these batteries will get me there.

#### APC Transfer Switch

A transfer switch is basically a power strip that allows for two simultaneous input sources (usually a UPS and wall power; I spoke to a Reddit user who described how plugging it in to two different line-interactive UPS's can cause bizarre problems and is not advisable). This is a very useful tool if you need to temporarily switch your server rack to wall power while you do battery maintenance, and I regret to inform you that this transfer switch already came in handy.

#### APC 1500VA UPS

I was very fortunate to snag an APC SMX1500RM2U from someone local using Facebook Marketplace. It was in great condition, the batteries were relatively recently installed, and I got a great deal. Honestly, I probably should have stopped here.

#### APC Battery Expansions

Unfortunately I got a little greedy in terms of batteries and ordered **two** battery expansions for my UPS. I found them both on eBay from the same seller for what I thought was a great deal: ~$400 each for a new, unused expansion bay.

The problem was that, while these were in fact unused and factory sealed, the batteries inside were from 2017; whether or not that counts as new became an argument between myself and the seller. I had to laern the hard way that sealed lead acid (SLA) batteries **need** to be changed out every three years, even if they are unused, otherwise if you attempt to use them, there is a high likelihood that the batteries will physically expand in such a way that damages their sleeve *and* causes them to give off a toxic gas. This happened with **both** units that I purchased, and I was lucky that I happened to be home both times this happened (roughly a month into owning and using them), otherwise I'd have come home to a dead dog. The gas was supposedly hydrogen sulfate which is 1) toxic, and 2) allegedly smells like rotten eggs, but both I and my partner think that it smells completely different and there is no close comparison that I can give you; it just smells like a really bad chemical leak.

I used this snafu as an opportunity to just go ahead and replace all the batteries in the whole stack just to be absolutely certain that everything in there was new and reputable. Since there are three units (UPS, expansion#1 and expansion#2), I started by just replacing the UPS batteries and will keep each expansion bay decomissioned until 2026 and 2027, respectively. Since UPS batteries need to be replaced every three years, I figured I would set myself up on an annual cadence for battery replacement; every August, something on the battery stack will need to get its batteries replaced, and I used a label maker to put the month and yaer on each device for when they will next need new batteries.

Speaking of batteries, there are a bunch of cheap "mystery meat" batteries that I found online that are sold as packs for the SMX1500. But given my already negative experience with batteries, I decided to leave nothing to chance and instead just went to my local battery store and got four Duracel SLA batteries that matchd the voltage and amp hours listed on APC's website for their first-party replacement batteries. The only odd thing is that, since replaceing those batteries and running successful diagnostics with no warnings, I get a periodic beep from the UPS almost hourly that didn't happen prior, and the logs are not particularly informative. Either way, I leave the office door closed now whenever I leave the house or go to bed so that any fumes that start spewing out of my server rack stay mostly trapped there.

# Future Additions

## SFP+ switch

I haven't purchased this yet, and will only do so if I decide that very high local network speed is worth the investment. This will also necessitate getting an SFP+ NIC for my NAS, ProxMox server, gaming PC, workstation, basically anything that would benefit from blisteringly fast data transfers.

## Network Video Recorder (NVR)

Like I mentioned above, I plan to replace my Nest cameras with locally-backed up IP cameras and initially plan to implement/test this on the Proxmox server. Once it is set up to my liking and I know how to configure the recording software, I will spin this off to dedicated hardware; my thinking is that critical infrastructure (and sercurity cameras are critical) should have dedicated hardware so that if something goes wrong, there are fewer steps required to identify and fix the issue, whereas virtualizing it introduces additional points of failure that increase investigation time.

## Hardware NAS and Just a Bunch of Disks (JBOD)

In keeping with "critical infrastructure should have dedicated hardware", I will eventually move my NAS off of the Proxmox cluster and onto it's own dedicated server, ideally one attached to a JBOD that supports 3.5" HDDs so I can increase the storage capacity.

# Conclusion and Followup

This has been my big project for the past few months and I'm glad I finally pulled the trigger on it. I intend to write out more verbose documentation for each piece of the homelab, partially to share with others but mostly for my own documentation so that I can replicate this in the future if I have to. I will update this post with links to the docs for each respective pice of hardware as I write them. I might also make all these docs into static links in the sidebar in the future, but I will revisit that once they are finished.

If you have any questions, tips, warnings, or recommendations, please use the reply link at the bottom of the page to send me a private response!

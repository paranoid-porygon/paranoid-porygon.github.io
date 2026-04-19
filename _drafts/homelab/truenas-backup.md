---
layout: post
title:  "How to backup TrueNAS to a hard disk"
summary: 
date:   
last_updated:
tags: [homelab]
published: false
---

If you are running your own storage solution in your home, you hopefully are already aware of the importance of regular and redundant backups. "You're one house fire/power surge/brownout away from catastrophe", "one is none, and two is one", "use the 3-2-1 plan", etc. You get it. Back your shit up using multiple offline backups, ideally with at least one stored off-site.

TrueNAS Scale is the storage software that I personally use (in case you haven't been following [my periodic homelab updates][homelab-blog]) and is probably the most popular open source DIY solution for a personal NAS, and this post will be a guide on how to copy an existing ZFS dataset on your TrueNAS to a single external HDD as a form of cold storage.

If you had enough technical know-how to setup TrueNAS, the steps in this guide should be easy enough to follow. This guide assumes that you (like me) are virtualizing your TrueNAS instance using the Proxmox hypervisor; if that is not the case, you may skip the Proxmox setup section.

# Hardware requirements

Here's what you will need to follow this guide:

* a "[hard drive toaster][hard-drive-toaster]", aka a USB-to-SATA adapter for a modern SATA HDD
* the ability to plug said toaster/adapter into your TrueNAS (or Proxmox) host's usb port
* (ideally multiple) individual HDD(s) 
    - the capacity of a single drive must be greater than the effective storage size of your TrueNAS storage pool that you intend to backup
    - e.g. I have eight 1Tb drives in a raidz2 configuration, giving me an effective capacity of just under 6Tb, so I will be backing up to a single 6Tb drive
    - I recommend WD Red drives for this purpose

# Proxmox setup

Plug in and power on USB-to-SATA adapter or hard drive toaster. Go to your TrueNAS VM and click the hardware tab. Add a device and enable full USB passthrough to the port where your hard drive toaster sis plugged in.

# TrueNAS setup

## Adding the disk to a new storage pool

The disk should show up in the TrueNAS web interface under Storage as a new disk.

When you click **Create Pool**, it might ask if you are sure you want to delete the data on the disk if it had previously been part of another pool. Confirm that you want to do so.

Download the encryption key somewhere safe. We will change this to passkey encryption shortly.

## Encrypt the root dataset

Click on the dataset for the pool you just made. You'll see the ZFS Encryption panel to the right. Inside that panel, click on the **Edit** button.

In the popup, use the drop down list to change the encryption type from "Key" to "Passphrase". I do this for ease of access, however it theoretically makes your dataset encryption more crackable and unable to be opened if you develop amnesia, dementia, or are otherwise incapacitated. Set your desired passphrase, leave the other options as default unless you know what you're doing, confirm, and save.

## Create a replication task

Now navigate using the main TrueNAS menu to **Data Protection**. At the very bottom of that page will be the **Replication Tasks** panel. You likely won't have any existing replication tasks, but in mine you will see an existing one from when I first set this up.

Let's duplicate what I already did for this new backup drive we're adding. Click on the **Add** button in that panel. On the **Replication Task Wizard** panel that comes up, click **Advanced Replication Creation** and copy the configuration I include below:

Make sure that under the destination dataset, you manually type the name of a dataset that you want to create during this replication task. For my use case, that looks like `backup_pool-01/backup`. The `backup` dataset will be created during replication, copying over all the data, settings, and snapshots from my actual storage pool.

It's up to your discretion whether or not to **Allow Compressed WRITE records**. I might disable this in the future for the sake of data integrity at the cost of backup speed.

Save once finished, and return to the **Data Protection** page. Let's enable our new replication taskand run it.

# Troubleshooting

When I first set this process up, I had a hell of a time figuring out the correct settings to make the replication actually happen. There were lots of issues dealing with encrypting the root vs child dataset, not creating a child dataset to copy to before the replication happens, how to properly copy snapshots, and other issues I likely forgot how to solve. The error messages that TrueNAS gives you are not as informative as you might want, and I figured out more via trial-and-error than by googling solutions. Hopefully this guide will allow you to forego the tribulations that I went through in setting up what you would think would be a relatively simple backup solution.

[homelab-blog]: 
[hard-drive-toaster]: 


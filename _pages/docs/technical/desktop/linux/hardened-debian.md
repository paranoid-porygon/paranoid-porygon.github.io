---
layout: default
title: "HOWTO: Hardened Debian Guide"
permalink: /docs/hardened-debian
---

* This will become a table of contents (this text will be scrapped).
{:toc}

# Hardened Debian

## Overview

This is meant as a straighforward guide to securing a GNU/Linux (specifically Debian-based) desktop against most likely security threats. It requires some level of technical knowledge in order to identify and diagnose when and where certain steps cannot be completed successfully, but you by no means need a degree or years of Linux experience to do that. Google is your friend, however double-check their AI-summary responses as I have found them to be out-of-date roughly 40% of the time. 

While this guide focuses on Debian (selected for its ease of setup, relative stability, off-the-shelf security features --apparmor being enabled by default--, and ubiquity of its package manager), the principles described here apply to all Linux systems, and the specific software can be found in most official repos, with a couple of exceptions (firejail, Mullvad).

Debian has unfortunately lagged in terms of modern package adoption and inclusion into their main repos. If you are technically skilled, please consider volunteering as a Debian maintainer.

I performed these steps on a Lenovo T480 so you may experience some hardware differences or peculiarities. I picked the T480 for a variety of reasons, one of which being the potential to replace the BIOS with Coreboot, Libreboot, or Heads should I choose to do so in the future. This may be desirable for you as well depending on your threat model; personally, I don't see why you would leave more security on the table when available, so you should do it if you are technically capable.

### Checklist

We want to make this computer reasonably, but not cumbersomely, secure. To do so we will prioritize the following:

1. password-protect the BIOS
2. install the base system to a fully-encrypted LVM 
3. TODO enable SecureBoot
4. blacklist unneeded kernel module
5. implementing MAC (Mandatory Access Control) through apparmor
6. implementing sandboxing using firejail
7. configuring a firewall using iptables/UFW
8. establishing corrective controls (clamav, rkhunter, deja-dup)
9. using USBGuard to protect against rogue USB devices
10. miscellaneous security measures
11. using Mullvad VPN to secure our traffic and DNS requests
12. setting up virtualization for disposable VMs when browsing sketchy sites
13. establishing detective controls (logwatch, lynis, AIDE)
14. utilizing hardware security tokens (yubikey)
15. TODO managing identity and access (KeePassXC, PGP)

### Future research

Explore installing Coreboot onto the T480.

In the future, I would like to attempt to use an ecrypted boot partition.

Look into intrusion detection using snort vs tripwire.

Consider moving to Wayland. In the meantime, set all browsers to use the --x11 or --xpra flags in their firejail configs.

Look into OpenSnitch as an application-level firewall.

#### Hardware Upgrades

##### Wi-Fi 6e or Wi-Fi 7 card

#### Privacy screen

* https://us.targus.com/products/4vu-magnetic-privacy-screen-lenovo-t460-t470-t480-asum001gl
* https://viascreens.com/screen-protector/lenovo/thinkpad-t480-non-touch/privacy
* https://www.amazon.com/Mamol-Laptop-Privacy-Screen-Thinkpad/dp/B0CP3MP6BV

#### Heads instead of coreboot/libreboot
* https://osresearch.net/

#### Replace Debian/CBPP with Qubes
* https://www.reddit.com/r/Qubes/comments/1bhnfrx/ist_the_thinkpad_t480_the_best_option/
* https://forum.qubes-os.org/t/thinkpad-t480-qubes-os-pre-install-steps-bios-decisions/37712/3
* https://forum.qubes-os.org/t/lenovo-thinkpad-t480/3290/61
* https://forum.qubes-os.org/t/building-a-fully-immutable-linux-os-image-fully-verified-with-your-own-secure-boot-key/34412


## 1    BIOS password

Before even installing the operating system, it's important to prevent unauthorized users from being able to boot the machine (either into our OS drive or an external live disk) or access the BIOS/UEFI and make nefarious changes. To this end, you'll want to enter the BIOS/UEFI menu at boot, usually by tapping F1, F2, or Delete right after the system posts.

Depending on your system, you'll have to explore the menu to find all the places where you can enable a password. I recommend enabling one everywhere you can but at a minimum for the following activities:

* accessing the BIOS menu
* accessing the boot selector

You can also prevent the computer from booting without a password if you would like, however this might be redundant since we will also be enabling full-disk encryption using a passphrase and optionally a bootloader password. In theory, a BIOS password prevents an adversary from being able to boot into external media, but there is already a way to mitigate this (see below).

### 1.1 Update firmware

Before taking any other steps, you should make sure that your machine's firmware/UEFI is updated to its latest version. How you do this will be specific to the make and model of your machine, however it will involve using another internet-connected computer and a blank USB flash drive big enough to hold the firmware image.

Be extremely cautious with this step. Flubbing it could render your machine unusable and unrecoverable.

### 1.2 Lock boot order after install

Prior to and during installation of your desired OS, you will want the BIOS/UEFI to prioritize booting from external (usually USB) media. Once you have intalled the OS, you'll want to come back to the BIOS/UEFI menu and reorder the boot order, placing the internal drive at the top and any USB or external media below it. I personally would just remove any external media from the boot order, but this might veer into paranoid territory since you've password-protected the boot selector anyways.

### 1.3 A note about hardware access

This is probably self-evident, but if an adversary has unfettered access to your machine and no time constraints, they can easily open up the computer chassis and reset the BIOS, the same way that a safe cracker can open any safe given enough time. The BIOS/UEFI password is a first-line mitigation tactic; think of it as keeping your guns in a gun safe so that a child can't get access to them. It's enough to slow a bad actor down to give you time to intervene.

## 2    [TODO] OS Installation to Encrypted LVM

### 2.1 Create an EFI parition

Manually create a 1Gb partition of type/filesystem "EFI system", and set the bootable flag.

### 2.2 Create a /boot partition

Manually create a 500Mb partition of type/filesystem ext2 and set the mount point as `/boot`. 

There are a variety of ways to set this up. The best option for our purposes is probably to use a removeable /boot partition since we will be deleting it after we get Secure Boot installed. However, there is a security argument to be made for keeping the /boot partition and it being removeable to prevent unauthorized boot into the machine. Choose whichever you think is best, but if you don't know enough to have a strong opinion, just make it removeable so that we can delete it later in this process. 

##### 2.2.a   Unencrypted /boot

For an unencrypted boot partition, simply create it the same way you did the EFI system partition on the same internal drive.

##### 2.2.b   Encrypted /boot

You can opt to encrypt your boot partition, however this is superfluous for our purposes since we will be setting up a unified kernal image and bypassing the bootloader altogether. Feel free to research this futher if it interests you, or if you don't plan to use or do not have Secure Boot.

##### 2.2.c   Removeable /boot

More-or-less the same as creating an unencrypted boot partition on the internal drive, just specify an unused USB drive instead during the partitioning process.

### 2.3 Create an encrypted LVM

We will use a passphrase rather than letting the TPM module handle decryption
	* https://tpm.fail/
	* https://pulsesecurity.co.nz/articles/TPM-sniffing

With the remaining disk space, create a partition meant for encryption. Then highlight it and select "prepare drive for encryption". Write pending changes to the disk and allow the partitioning tool to write random data to the disk to prevent snooping.

After the encrypted partition has been created, select it and click "prepare logical volume". Create and name a new logical volume.

#### 2.3.1  Suggested Volume Layout

While you are perfectly fine just making the entire encrypted partition act as your root partition and not specifying any others, provisioning specific mountable partitions can help prevent your disk being filled either due to bad storage hygiene or malware. This is the layout that I recommend:

* / - 64Gb
* /var - 16Gb
* /tmp - 2Gb
* swap - equal to your RAM capacity or 16Gb, whichever is smaller
* /home - the rest of the disk (100%)

All of these, with the exception of swap, should be type ext4.

If you are familiar with btrfs or zfs, feel free to use those. I'm going with ext4 because it has more years behind it.

### 2.4 Continue with installation

Once you have finalized the encrypted LVM, you are ready to move on and complete your OS installation. Finish the install, remove the installation media, and reboot.

If the machine does not boot, check your UEFI menu and ensure that Secure Boot is disabled and that your internal drive that the OS was installed to is listed in the boot order.

## 3    [TODO] Enable SecureBoot

* https://threere.com/guides/secure-debian/
  - https://threere.com/guides/secure-debian/0-installation/
  - https://threere.com/guides/secure-debian/0-usb-installation-specifics/
  - https://threere.com/guides/secure-debian/1-secure-boot/
    * https://wiki.archlinux.org/title/Unified_kernel_image
* https://wiki.debian.org/SecureBoot

** *NOTE: I did not get this working properly. Proceed at your own discretion. The machine will work fine if you skip this whole section.* **

### 3.1 Installing necessary pkgs

```
sudo apt install sbsigntool efitools mokutil efibootmgr systemd-boot
```

### 3.2 Create certificates

The following commands must be run as root:

```
sudo -i
```

Create the cert directory:

```
mkdir -p /var/lib/shim-signed/mok/
cd /var/lib/shim-signed/mok/
```

Generate a new RSA 4096 key to sign the kernel; if this fails, try using a 2048 key instead

```
openssl req -nodes -new -x509 -newkey rsa:4096 -keyout MOK.priv -outform DER -out MOK.der -days 36500 -subj "/CN=Local Linux/"
openssl x509 -inform der -in MOK.der -out MOK.pem
```

Copy the DER cert to the EFI partition

```
cp /var/lib/shim-signed/mok/MOK.der /boot/efi/
```

### 3.3 Create a unified kernel image

Install systemd-ukify:

```
apt install systemd-ukify
```

Get the root disk UUID:

```
sudo blkid
```

Manually copy it to your clipboard.

Create `/etc/kernel/cmdline` and add:

`root=UUID=<<UUID>> panic=0 ro quiet splash`

Create `/etc/kernel/postinst.d/zz-update-efistub` and add:

```
#!/bin/bash

INITRD=/initrd.img
INITRD_NAME=$(basename $(readlink ${INITRD}))
UNAME=${INITRD_NAME//initrd.img-/}

ukify build \
    --os-release @/usr/lib/os-release \
    --cmdline @/etc/kernel/cmdline \
    --linux /vmlinuz \
    --initrd ${INITRD} \
    --uname ${UNAME} \
    --output "/boot/efi/EFI/my-debian/Linux.efi"

if [ $? -ne 0 ]; then
    echo "Failed to build the unified kernel image"
    exit 1
fi

sbsign \
    --key /var/lib/shim-signed/mok/MOK.priv \
    --cert /var/lib/shim-signed/mok/MOK.pem \
    --output /boot/efi/EFI/my-debian/Linux.efi \
    /boot/efi/EFI/my-debian/Linux.efi

if [ $? -ne 0 ]; then
    echo "Failed to sign the unified kernel image"
    exit 1
fi
```

The above script will also need to be run whenever the initramfs is updated, so do the following:

```
mkdir -p /etc/initramfs/post-update.d
ln -s /etc/kernel/postinst.d/zz-update-efistub /etc/initramfs/post-update.d/zz-update-efistub
chmod +x /etc/kernel/postinst.d/zz-update-efistub
```

And finally, generate the unified kernel:

```
mkdir /boot/efi/EFI/my-debian
/etc/kernel/postinst.d/zz-update-efistub
```

### 3.4 Add boot entry

```
efibootmgr --disk /dev/nvme0n1 --create --label "Debian" --loader '\EFI\my-debian\Linux.efi'
```

### 3.5 Enroll keys

Reboot the machine, go into the UEFI menu, go to wherever your Secure Boot settings are located, and put the machine in setup mode. Save and reset.

If everything went correctly, the machine should then boot into your OS.

### 3.6 Setup DKMS

```
sudo apt install dkms
```

Then edit the `/etc/dkms/framework.conf` file, adding the following:

```
mok_signing_key="/var/lib/shim-signed/mok/MOK.priv"
mok_certificate="/var/lib/shim-signed/mok/MOK.der"
```

### 3.7 Add option to boot into old kernel image

If there are issues booting into the system after a kernel update, it'll be useful to have an old kernel as a backup.

Add the following to `/etc/kernel/postinst.d/zz-update-efistub`:

```
# Create Old Image
INITRD=/initrd.img.old
INITRD_NAME=$(basename $(readlink ${INITRD}))
UNAME=${INITRD_NAME//initrd.img.old-/}

ukify build \
    --os-release @/usr/lib/os-release \
    --cmdline @/etc/kernel/cmdline \
    --linux /vmlinuz.old \
    --initrd ${INITRD} \
    --uname ${UNAME} \
    --output "/boot/efi/EFI/my-debian-old/Linux.efi"

if [ $? -ne 0 ]; then
    echo "Failed to build the unified kernel image (old)"
    exit 1
fi

sbsign \
    --key /var/lib/shim-signed/mok/MOK.priv \
    --cert /var/lib/shim-signed/mok/MOK.pem \
    --output /boot/efi/EFI/my-debian-old/Linux.efi \
    /boot/efi/EFI/my-debian-old/Linux.efi

if [ $? -ne 0 ]; then
    echo "Failed to sign the unified kernel image (old)"
    exit 1
fi
```

Create the EFI directory for the fallback kernel and run the script to generate the image:

```
mkdir -p /boot/efi/EFI/my-debian-old

/etc/kernel/postinst.d/zz-update-efistub
```

Finally, add the fallback image to UEFI:

```
efibootmgr --disk /dev/nvme0n1 --create --label "Debian Old" --loader '\EFI\my-debian-old\Linux.efi'
```

### TODO 3.8 Remove Grub

GRUB is now redundant since the UEFI can boot the unified kernel image directly.

```
apt remove --purge grub-efi-amd64
apt autoremove --purge
```

The above might not work, which is fine. We can just remove the GRUB entries from `/boot/efi`:

```
# remove the one for your machine id
rm -rf /boot/efi/<<youre-hex-machine-id>>/
rm -rf /boot/efi/EFI/debian
rm -rf /boot/grub
```

If there is a residual Debian entry in your UEFI boot manager, you can remove that as well. Check to see if it's there using:

`efibootmgr`

Find the correct boot id (i.e. `Boot0000`) and run:

`efibootmgr -b <<num>> -B`

### TODO 3.9 Remove the /boot partition

Comment out the line in `/etc/fstab` that mounts the /boot parition:

`sudo vim /etc/fstab`



### TODO 3.10    Move /boot/efi to /efi

## 4    Blacklist bluetooth, firewire, and thunderbolt kernel module

Create `/etc/modprobe.d/blacklist.conf` and add the following:

```
blacklist btusb
blacklist bluetooth
install bluetooth /bin/false
blacklist firewire-core
blacklist thunderbolt
```

Then disable the bluetooth service.

```
sudo systemctl stop bluetooth
sudo systemctl disable bluetooth
sudo systemctl mask bluetooth
```

Reboot to apply settings.

Alternatively/additionally, disable bluetooth by removing the hardware radio.

## 5    Mandatory Access Control (apparmor)

```
sudo apt update
sudo apt install apparmor apparmor-profiles apparmor-profiles-extra apparmor-utils
sudo systemctl enable apparmor
sudo systemctl start apparmor
```

Kernel module should be enabled by default, unlike in Arch, so no need to modify grub.

You can verify that apparmor is running by executing:

```
sudo aa-enabled
```

## 6    Sandboxing (firejail)

### 6.1 Building from source

Firejail is not in the Debian repos and therefore needs to be installed from source in order to enable apparmor cofunctionality.

* https://firejail.wordpress.com/download-2/
* https://github.com/netblue30/firejail#building

#### 6.1.1 Dependencies and build tools

```
sudo apt-get update -qy
sudo apt-get install -qy \
  git build-essential fakeroot lintian libapparmor-dev pkg-config gawk
```

#### 6.1.2  Cloning and building from source

Create a dir in ~/git-repos for filejail.

```
git clone 'https://github.com/netblue30/firejail.git'
cd firejail
```

Install like this to enable apparmor as well:

```
./configure --enable-apparmor && make && sudo make install-strip
```

### 6.2 Enable systemwide usage

```
firecfg --fix-sound
```
 
Then log out and log back in to run:

```
sudo firecfg
```

### 6.3 Create app profiles as needed

Default profiles are located in `/usr/local/etc/firejail`.

#### 6.3.1  Firefox
* https://firejail.wordpress.com/documentation-2/firefox-guide/


##### 6.3.1.1 Per-usage profile

It may be useful (and advisable) to create separate Firefox profiles for different use cases, i.e. personal use, work, OSINT, etc. In particular, you might want to selectively whitelist which download folders each profile has access to.

A example command for running firefox with a given firejail profile and firefox profile might look like this:

```
firejail --profile=/path/to/firejail/profile firefox -Profile=/path/to/firefox/profile/dir -no-remote
```

Even better would be to create a desktop launcher for your profiles.

```
vim ~/Desktop/firefox-<<profile>>.desktop
```

And paste something like this:

```
[Desktop Entry]
Name=firefox-<<profile>>
Icon=/opt/firefox/browser/chrome/icons/default/default48.png
Terminal=false
Type=Application
Exec=firejail --profile=~/.config/firejail/firefox_<<profile>>.local firefox -profile "/home/user/.mozilla/firefox/<<profile>>" -no-remote
Comment=
Path=
StartupNotify=false
```

You might nee dto change `/opt/firefox/firefox` depending on where your firefox binary is located.

#### 6.3.2  Chromium

#### 6.3.3  Signal

#### 6.3.4  ping

## 7    Firewall (gufw)

* https://wiki.archlinux.org/title/Uncomplicated_Firewall

I'm lazy so we're just going to use gufw: graphical uncomplicated firewall. This will automatically generate nftables for us (iptables is on the way out).

### 7.1 Installation and setup

```
sudo apt install gufw
sudo systemctl enable --now ufw
sudo ufw enable
```

You can then use gufw to check the settings. I set mine to public all the time.

### 7.2 VPN settings

If ufw ends up blocking you from connecting to your VPN, check this page for help:
https://wiki.archlinux.org/title/Uncomplicated_Firewall#Forward_policy

### 7.3 Firewall Rules

In gufw, create a new profile for DEFCON with the following rules:

* Incoming: Deny
* Outgoing: Allow
* Custom:

Then verify by running:
`sudo ufw status verbose`

### 7.4 Closing ports and disabling services

To see all open ports, run `ss -l`.

To show all listening processes and their numeric tcp and udp port numbers, run:
`ss -lpntu`

#### 7.4.1  Disable sshd

This is a client, not a server. There is no reason for sshd to be running.

```
sudo systemctl disable --now sshd
sudo systemctl disable --now ssh
```

## 8    Corrective Controls (anti-malware, backup restoration)

TODO merge section 14 into this

### 8.1 Anti-malware

```
sudo apt install clamav rkhunter
```

Up to the user if they want to set these scans up to run on a cron job.

#### 8.1.1 ClamAV

ClamAV is the most commonly used Linux antivirus solution. It can scan your system and provide warnings and alerts if it detects any known infections.

##### 8.1.1.1  WIP Run as a daemon

This is extremely memory-intensive and generally not advised for anemic hardware. You are better off just running ad-hoc or periodic scans when the machine is not in use, making sure that the machine is on AC power and the cooling fan works.

```
systemctl start clamav-daemon
```

##### 8.1.1.2  On-demand scanning

Before any on-demand or ad-hoc scan, make sure to update virust definitions/signatures:

```
sudo freshclam
```

###### 8.1.1.2.1   Whole-system scan

```
clamscan --recursive /
```

###### 8.1.1.2.2   Single file scan

```
clamscan filename.ext
```

###### 8.1.1.2.3   Single directory scan

```
clamscan --recursive /path/to/dir
```

#### 8.1.2 rkhunter

rkhunter is a tool for detecting the presence of known rootkits on the host machine. It can be run ad hoc or periodically using the following command:

```
sudo rkhunter --update
sudo rkhunter --propupd
sudo rkhunter --check --sk
```

### 8.2  Backups 

* https://wiki.archlinux.org/title/System_backup
  - https://wiki.archlinux.org/title/Synchronization_and_backup_programs#Chunk-based_increments
    * https://wiki.archlinux.org/title/Duplicity

There are two types of backups that I recommend: full-disk copies and incremental backups.

#### 8.2.1 TODO Full-disk copy (dd)

Copying an entire disk block-by-block is useful if you are about to perform potentially system-breaking changes. I perform this style of backup by booting into a live USB environment on the machine with the drive I want to back up, and then use the `dd` command to copy the internal drive to a blank external USB drive.

```

```

#### 8.2.2    Incremental backup (deja-dup)

Deja Dup is a graphical backup utility built on top of the `duplicity` backup utility, which itself is built on top of `rsync`. 

##### 8.2.2.1 Installation

```
sudo apt update && sudo apt install deja-dup
```

##### 8.2.2.2 Usage

Deja Dup is a graphical application. Just run it with:

```
deja-dup
```

Make sure you have an external drive that it can back up to, and then set up a backup location and schedule in the menu.


## 9   USBguard

* https://wiki.archlinux.org/title/Security#Protect_against_rogue_USB_devices
* https://wiki.debian.org/USBGuard

### 9.1    Installation and setup

```
sudo apt update && sudo apt install usbguard usbguard-notifier
sudo usbguard generate-policy | sudo tee /etc/usbguard/rules.conf
sudo systemctl enable --now usbguard
```

#### 9.1.1 Updating permanent rules

Let's say you have a usb hub or KVM switch that you want to attach to the computer and approve all devices plugged in to that switch.

You'll first need to ad-hoc whitelist each individual device:

```
sudo usbguard list-devices
sudo usbguard allow-device <id>
```

Once you have done that for the hub and each attached device, you'll then have to create the new ruleset and reload the usbguard daemon:

```
sudo usbguard generate-policy | sudo tee /etc/usbguard/rules.conf
sudo systemctl restart --now usbguard
```

### 9.2    Ad hoc rule changes

#### 9.2.1    Whitelisting a new device

View device list: 

```
sudo usbguard list-devices
```

Allow a specific device: 

```
sudo usbguard allow-device <id>
``` 

#### 9.2.2 Blacklisting a device

Block a device: 

```
sudo usbguard block-device <id>
```

#### 9.2.3 Allow all connected devices

This is useful when you have plugged in a known USB hub or dock.

To authorize all connected devices:

```
for d in /sys/bus/usb/devices/*/authorized; do echo 1 > $d; done
```

### 9.3    Setting up the notifier

* https://manpages.debian.org/testing/usbguard-notifier/usbguard-notifier.1.en.html

```
sudo usbguard add-user USER -d listen
```

or

```
sudo usbguard add-user -g GROUP -d listen
```

then:

```
sudo systemctl restart usbguard
systemctl enable --now --user usbguard-notifier.service
```

### 9.4    Temporarily disable

* https://github.com/USBGuard/usbguard/issues/367
* https://www.reddit.com/r/linuxquestions/comments/1gh5n06/locked_out_by_usbguard_please_help/
* https://www.kicksecure.com/wiki/USBGuard

To temporarily disable (e.g. if you are cannibalizing parts from one laptop to put in another), run the following:

```
sudo systemctl disable --now usbguard
sudo systemctl disable --now usbguard-dbus
```

Once ready, just follow the setup steps above again.

### 9.5    Enable Kernel DMA Protection
* https://www.reddit.com/r/thinkpad/comments/ghph5p/psa_if_you_dont_use_thunderbolt_or_security_is_a/



## 10   Miscellaneous Security Measures

### 10.1    TCP/IP stack hardening
* https://wiki.archlinux.org/title/Sysctl#TCP/IP_stack_hardening

#### 10.1.1 Configuration

For this, we will need to create a drop-in file in `/etc/sysctl.d/`:

```
sudo vim /etc/sysctl.d/98-ip-stack-hardening.conf
```

My file looks like this:

```
# https://wiki.archlinux.org/title/Sysctl#TCP/IP_stack_hardening

# TCP SYN cookie protection

net.ipv4.tcp_syncookies = 1

# Protect against tcp time-wait assassination hazards, drop RST 
# packets for sockets in the time-wait state. 

net.ipv4.tcp_rfc1337 = 1

# By enabling reverse path filtering, the kernel will do source validation of the packets received from all the interfaces on the machine. This can protect from attackers that are using IP spoofing methods to do harm.
# 
# The kernel's default value is 0 (no source validation), but systemd ships /usr/lib/sysctl.d/50-default.conf that sets net.ipv4.conf.all.rp_filter to 2 (loose mode)
#
# The following will set the reverse path filtering mechanism to value 1 (strict mode):

net.ipv4.conf.*.rp_filter = 1
-net.ipv4.conf.all.rp_filter

# Disable ICMP redirects
# https://askubuntu.com/questions/118273/what-are-icmp-redirects-and-should-they-be-blocked
# To disable ICMP redirect acceptance: 
#
net.ipv4.conf.*.accept_redirects = 0
-net.ipv4.conf.all.accept_redirects
net.ipv6.conf.*.accept_redirects = 0
-net.ipv6.conf.all.accept_redirects
# To disable ICMP redirect sending when on a non router: 
#
net.ipv4.conf.*.send_redirects = 0
-net.ipv4.conf.all.send_redirects

# To disable ICMP echo (aka ping) requests: 
#
net.ipv4.icmp_echo_ignore_all = 1
net.ipv6.icmp.echo_ignore_all = 1
```

#### 10.1.2 Load config

Once finished adding drop-ins, execute the following to load all configs manually:

```
sudo sysctl --system
```

### 10.2    Forward root mail to other email address

Edit `/etc/aliases` to have the following:

```
root:		recipient@example.com
```

After which run `newaliases`. Some email providers will reject email coming in from nonexistent or non-routable domain names. If that is the case, you will need to play with your mail forwarding configuration until this actually works.

## 11   Mullvad VPN

At the risk of rehashing a sales pitch that has been made elsewhere: Mullvad VPN is, among commercial VPNs that you can pay for, the one that respects your privacy the most. There is a big caveat to that: by nature of how commercial VPNs that you didn't set up work, this is not a zero-trust solution for privacy or anonymity. If you want a more private and more anonymous solution, use TOR or, even better, I2P; both of these come with increased latency and other issues, and are also not perfect. But for the purpose of securing your internet traffic at DEFCON, on your home ISP, or at the library/coffee shop/hotel, a commercial VPN is sufficient, and Mullvad is the best.

Mullvad gives you the option of paying with cash, e-currencies, or credit cards. You create a random UUID as your account number (you don't actually have a login) and can just send cash in an envelope with your ID on a piece of paper to pay your dues. Personally, I just use a privacy.com card.

### 11.1    What VPNs do

VPNs are useful for a variety a security-enhancing applications, including (and mostly limited to):
* creating a secure tunnel so that your ISP and the LAN your computer is on cannot see your traffic; while Marcus Hutchins and others assert that this is superfluous in the age of SSL, not every site has SSL enabled and not every network service is HTTPS, so always use a VPN on public WiFi, especially on adversarial networks (i.e. DEFCON)
* DAITA can further shape your traffic to prevent deep-packet inspection
* obfuscating your internet IP address so that the sites and services you use can't tell where you are actually located (note that some sites are wise to this and block or limit VPNs in a variety of ways)
* providing a secure DNS that doesn't leak your DNS queries
* circumventing censorship on certain networks and in certain regimes

#### 11.1.1 What VPNs don't do

There is a lot of false advertising about what VPNs can do for customers, to the point that customers have invented their own lore about the security benefits of VPNs. For the record, VPNs **cannot**:

* secure or encrypt your emails once they have reached their destination
* prevent unauthorized access to your accounts if you password was compromised or part of a leak/breach
* allow you to watch Netflix from another country (again, sites are increasingly wise to the IP blocks used by VPNs and block or limit their access)
* replace a password manager or the need for making unique, secure passwords and implementing MFA
* protect your machine from malware, downloading random shit online, or otherwise flaunting basic computer hygiene
* hide from the VPN who you are if you are frequently visiting sites that can outright identify you
* shield you from law enforcement if the VPN provider is subpoenaed

### 11.2    Installation
* https://mullvad.net/en/help/install-mullvad-app-linux

#### 11.2.1 Dependencies

```
sudo apt install curl
```

#### 11.2.2 Download Mullvad's signing key

```
sudo curl -fsSLo /usr/share/keyrings/mullvad-keyring.asc https://repository.mullvad.net/deb/mullvad-keyring.asc
```

#### 11.2.3 Add the stable repository

This enables Mullvad to get updated every time we do a system-wide update.

```
echo "deb [signed-by=/usr/share/keyrings/mullvad-keyring.asc arch=$( dpkg --print-architecture )] https://repository.mullvad.net/deb/stable stable main" | sudo tee /etc/apt/sources.list.d/mullvad.list
```

#### 11.2.4 Install the package

```
sudo apt update
sudo apt install mullvad-vpn
```

### 11.3    Recommended settings

**DAITA:**                  Enable if you are on AC power and are worried about deep packet inspection or being found out as using a VPN; will be useful in places where VPNs are banned

**Multihop:**               Increases anonymity, decreases speed; useful in places with high censorship for when doing activities that might draw attention from onlookers or the site owner

**Launch on start-up**:     Enable

**Auto-connect**:           Enable

**Local network sharing:**  Fine on your home network; disable on adversarial or public networks; not a replacement for a firewall or disabling unnecessary ports

**DNS content blockers:**   Useful for safer browsing, but precludes custom DNS use

**Use custom DNS server:**  I just disable this and use Mullvad's DNS; if you have issues, try using Quad-9 aka 9.9.9.9 instead

**In-tunnet IPv6:**         Leave at default (disabled)

**Kill switch:**            Enable

**Lockdown mode:**          Enable on adversarial networks; safe to disable on home networks

**Anti-censorship:**        Automatic

**Quantum-resistant tunnel:**   Enable

**Device IP version:**      IPv4

Additionally, make sure to connect to a server location that is known not to censor the web. Chicago, IL is currently my go-to.

### 11.4    Split-tunneling

If you need an application to bypass the VPN, you can do this through the split-tunneling option. Only do this if you aren't on an adversarial network or know what you are doing.

## 12   Virtualization and Containerization

There are certain circumstances where you don't want to do something on your bare metal machine: malware analysis, certain OSINT hunts, opening files of unknown origin. For those situations, we will use virtualization.

While KMV and virt-manager are technically better options for Linux, Virtualbox is more widely used across operating systems and by my own colleagues that use Mac and (God help them) Windows, so that's what I will outline in this document.

We'll also go over different options for guest VMs and what operating systems make sense as guests.

### 12.1    Virtualbox

#### 12.1.1 Installation

* https://www.virtualbox.org/wiki/Linux_Downloads

Add the repo, download and register the signing key, and install virtualbox:

```
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian trixie contrib" | sudo tee /etc/apt/sources.list.d/oracle.list
wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg --dearmor
sudo apt-get update
sudo apt-get install virtualbox-7.2
```

*NOTE: check to see what the current version of VirtualBox is. It was 7.2 at time of writing.*

##### 12.1.1.1 Disable KVM kernel modules

While KVM is useful for virt-manager, Virtualbox whines about it if it's loaded.

Disable the relevant kernel modules:

```
sudo modprobe -r kvm_{intel,amd}
sudo modprobe -r kvm
```

And verify that they were unloaded:

```
lsmod | grep kvm*
```

We'll also need to blacklist the kvm module so that it doesn't load at boot. 

```
sudo vim /etc/modprobe.d/blacklist-for-virtualbox.conf
```

and add:

```
blacklist kvm
blacklist kvm_intel
blacklist kvm_amd
```

and make sure these changes take effect at boot:

```
sudo update-initramfs -u
```
##### 12.1.1.2  Install Virtualbox Extension Pack

Certain features like USB 3 passthrough and virtual disk encryption require the VirtualBox Extensions Pack.

Download it here: https://www.virtualbox.org/wiki/Downloads

Verify the integrity of the download against the checksum here: https://download.virtualbox.org/virtualbox/7.2.10/SHA256SUMS

```
sha256sum <<name-of-download>>
```

On newer versions of VirtualBox, there's a sidebar meny with an item called "extensions". Click on that, and then click on "Install" and navigate to where your downloaded and verified extensions file is located.

###### 12.1.1.2.1 Extensions fail to install

Installing the extension pack will likely fail using the GUI unless you run VirtualBox as root, which I do not recommend. It's better to just run the VBoxManage tool as root for this specific purpose and run VirtualBox itself as a user.

To install the extension pack using the cli, run:

```
sudo VBoxManage extpack install --replace /path/to/Oracle_VirtualBox_Extension_Pack.vbox-extpack
```

#### 12.1.2    TODO Virtual machine guests and operating systems

General recommendations for VM setup:

* encrypt the disk
* at least 4Gb of memory
* at least 32Mb of VRAM
* at least 2 CPU cores
* at least 16Gb of storage for linux VMs

##### Kicksecure

##### Tails

##### Whonix

###### Download the pre-built VM

* https://www.whonix.org/wiki/VirtualBox

###### Validating using checksum and Whonix signature

##### Nix

## 13   Detective controls

At this point, we have a reasonably secured system. We want to use this configuration as a baseline to compare future machine states against to identify if an unwanted or nefarious change has occurred.

### 13.1    Log monitoring (logwatch)

* https://wiki.archlinux.org/title/Logwatch
* https://oneuptime.com/blog/post/2026-03-02-how-to-set-up-log-monitoring-with-logwatch-on-ubuntu/view

#### 13.1.1 Installation and configuration

Install logwatch and then copy the default config file to the /etc/logwatch/conf folder for modification.

```
sudo apt install logwatch
sudo cp /usr/share/logwatch/default.conf/logwatch.conf /etc/logwatch/conf/.
sudo vim /etc/logwatch/conf/logwatch.conf
```

and set the following parameters:

```
Output = file
Filename = "/var/log/logwatch"
Detail = High
```
#### 13.1.2 Execution

```
sudo logwatch
```

This will generate a one-time report based on the configuration we specified above. Only root and sudoers will have the ability to view the output file.

### 13.2    Security auditing and baseline creation (lynis)

* https://cisofy.com/lynis/

> Lynis is a battle-tested security tool for systems running Linux, macOS, or Unix-based operating system. It performs an extensive health scan of your systems to support system hardening and compliance testing. The project is open source software with the GPL license and available since 2007.

#### 13.2.1 Installation

```
sudo apt update && sudo apt install lynis
```

#### 13.2.2 Usage

```
sudo lynis audit system
```

Reports and logs are then found in `/var/log/lynis-report.dat` and `/var/log/lynis.log` respectively.

### 13.3    AIDE for intrusion detection

* https://github.com/aide/aide
* https://aide.github.io/
* https://wiki.archlinux.org/title/AIDE
* https://aide.github.io/doc/

> AIDE is a tool for monitoring file system changes. It can be used to detect unauthorized monitored files and directories. AIDE was written to be a simple and free alternative to Tripwire.

> AIDE only does file integrity checks. It does not check for rootkits or parse logfiles for suspicious activity, like some other HIDS (such as OSSEC) do. For these features, you can use an additional HIDS (see [1] for a possibly biased comparison), or use standalone rootkit scanners (rkhunter, chkrootkit) and log monitoring solutions (logwatch, logcheck). 

#### 13.3.1 Installation

```
sudo apt install aide
```

#### 13.3.2 Configuration

To check your configuration, run:

```
aide -D
```

If you haven't run `aideinit` already, this will throw and error.

#### 13.3.3 Usage 

##### 13.3.3.1  Establishing a baseline

```
sudo aideinit
```

This may take some time to complete.

###### 13.3.3.1.1   Baseline integrity

Storing the baseline on the system being monitored runs the risk of the database itself being maliciously modified to hide attackers' tracks. After the database is created, copy it to external media and use that to compare against.

##### 13.3.3.2  Compare current state to baseline

```
aide -C
```

###### 13.3.3.2.1	Automating the baseline check
* https://github.com/OneUptime/blog/blob/master/posts/2026-03-04-schedule-automated-aide-integrity-checks-cron-rhel-9/README.md

Intrustion protection isn't worth much if you don't automate the check. Ideally the system should be checked every night when the machine isn't in use.

Create a wrapper script in `/usr/local/sbin/aide-check.sh`:

```
#!/bin/bash
# AIDE automated integrity check script
# Runs aide --check and logs results with timestamps

LOGDIR="/var/log/aide"
LOGFILE="${LOGDIR}/aide-check-$(date +%Y%m%d-%H%M%S).log"
MAILTO="root"
MAIL_CMD="/usr/bin/s-nail"

# Make sure log directory exists
mkdir -p "${LOGDIR}"

# Verify that machine is on AC power, otherwise log that it wasn't
ac_file=$(find /sys/class/power_supply/ -name "online" | head -n 1)

if [[ -f "$ac_file" && $(cat "$ac_file") -eq 1 ]]; then
	# Run the check and capture both output and exit code
	echo "AIDE check started: $(date)" > "${LOGFILE}"
	echo "---" >> "${LOGFILE}"

	/usr/sbin/aide --check >> "${LOGFILE}" 2>&1
	EXIT_CODE=$?

	echo "---" >> "${LOGFILE}"
	echo "AIDE check finished: $(date)" >> "${LOGFILE}"
	echo "Exit code: ${EXIT_CODE}" >> "${LOGFILE}"
else
	echo "AC not plugged in. AIDE check skipped." > "${LOGFILE}"
	EXIT_CODE=0
fi

# AIDE exit codes:
# 0 = no changes
# 1-7 = changes detected (added, removed, changed, or a combination)
# 13+ = errors
if [ ${EXIT_CODE} -ne 0 ]; then
    # Send the report via mail
    "${MAIL_CMD}" -s "AIDE Alert: Changes detected on $(hostname)" "${MAILTO}" < "${LOGFILE}"
fi

# Clean up logs older than 90 days
find "${LOGDIR}" -name "aide-check-*.log" -mtime +90 -delete

exit ${EXIT_CODE}
```

And then make the script executable:

```
sudo chmod 700 /usr/local/sbin/aide-check.sh
```

And finally update cron:

```
sudo crontab -e
```

And, using your preferred text editor, add the following lines:

```
# Daily AIDE integrity check at 3 AM
0 3 * * * /usr/local/sbin/aide-check.sh
```

Alternatively, create `/etc/cron.d/aide-check` and add:

```
# AIDE file integrity check - runs daily at 3:00 AM
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
0 3 * * * root /usr/local/sbin/aide-check.sh
```

And set the perms:

```
sudo chmod 644 /etc/cron.d/aide-check
```

###### 13.3.3.2.2	Managing check duration

AIDE checks can take a while on large systems. You can measure the duration:

```bash
# Time a manual AIDE check
sudo time aide --check
```

If checks take too long, consider:

```bash
# Run with nice to lower CPU priority in /etc/cron.d
0 3 * * * root nice -n 19 /usr/local/sbin/aide-check.sh

# Run with ionice to lower I/O priority in /etc/cron.d
0 3 * * * root ionice -c 3 nice -n 19 /usr/local/sbin/aide-check.sh
```

##### 13.3.3.3  Update the baseline

```
aide -u
```

*NOTE: This should be the last step you take in setting up your secured machine as AIDE will establish this baseline based on your installed packages.*

###### 13.3.3.3.1	Auto-updating after each system update

You will additionally want to add this towards the end of your system update script, right before asking to reboot.


## 14   Hardware Security Token (Yubikey)

### 14.1 Installation

#### 14.1.1 Install Dependencies

```
sudo apt install pcscd
```

##### 14.1.1.1 Verify that the pcscd service is running

```
sudo systemctl enable --now pcscd
```

#### 14.1.2 Add Yubi developer keys to GPG keyring

This whole processed is outlined [on the Yubico website](https://developers.yubico.com/Software_Projects/Software_Signing.html) and includes current developer key fingerprints. For the sake of demonstration, I'll include the command to import all the current keys as of time of writing this:

```
gpg --recv-keys \
0a3b0262bca1705307d5ff06bca00fd4b2168c0a \
20ee325b86a81bcbd3e56798f04367096fba95e8 \
b70d62aa6a31ad6b9e4f9f4bdc8888925d25ca7a \
b6042e2bd1fdbc2bca8588b2ff8d3b45b7b875a9 \
57a9deed4c6d962a923bb691816f3ed99921835e \
268583b64786f50f807456da8ced3a80d41c0dcb \
1d7308b0055f5aef36944a8f27a9c24d9588ea0f \
355c8c0186cc96cba49f9cd8daa17c2953914d9d \
9E885C0302F9BB9167529C2D5CBA11E6ADC7BCD1 \
7FBB6186957496D58C751AC20E777DD85755AA4A \
78d997d53e9c0a2a205392ed14a19784723c9988 \
AF511D2CBC0F973E5D308054325C8E4AE2E6437D \
C28ED3753F01B4B097A1B306948B29C5F1E063ED \
F475928225229F58460640EAD91777C175533EE5
```

#### 14.1.3 Download the installation files and signature

Change to the directory where you want to download the source and signatures and then run:

```
wget developers.yubico.com/yubioath-flutter/Releases/yubico-authenticator-latest-linux.tar.gz
wget developers.yubico.com/yubioath-flutter/Releases/yubico-authenticator-latest-linux.tar.gz.sig
```

#### 14.1.4 Check the signature

In that directory, run:

```
gpg --verify yubico-authenticator-latest-linux.tar.gz.sig
```

The output *should* contain the phrase `Good signature from` and the `Primary key fingerprint` and might look something like this:

```
gpg: assuming signed data in 'yubico-authenticator-latest-linux.tar.gz'
gpg: Signature made Wed Jul  8 08:39:55 2026 EDT
gpg:                using RSA key 20EE325B86A81BCBD3E56798F04367096FBA95E8
gpg: Good signature from "Dain Nilsson <dain@yubico.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 20EE 325B 86A8 1BCB D3E5  6798 F043 6709 6FBA 95E8
```

You can safely ignore the warning that `key is not certified with a trusted signature` so long as you compare the Primary key fingerprint against those listed on yubico's website.

#### 14.1.5 Extract the files

```
sudo mkdir /opt/yubico-authenticator
sudo tar -xvzf yubico-authenticator-latest-linux.tar.gz -C /opt/yubico-authenticator --strip-components 1
sudo ln -s /opt/yubico-authenticator/authenticator /usr/local/bin/yubico-authenticator
```

### 14.2 Running the authenticator

```
./desktop_integration.sh --install
yubico-authenticator
```

### 14.3 Adding an ssh key

* https://techearl.com/ssh-key-yubikey-fido2
 
Insert the key and run the following:

```sh
ssh-keygen -t ed25519-sk -C "you@example.com"
```

It will prompt you for the following:
* location to save pub key and key handle; I save these to my syncthing folder
* whether or not you want to use a passphrase in addition to having to touch the key

### Integrating with KeePassXC

#### Updating Firejail Profile

## 15 KeePassXC Password Manager

### 15.1 Installation

```
sudo apt install keepassxc
```

### 15.2 Settings

#### 15.2.1 Basic Settings

If not listed below, leave as default.

* Start only a single instance of KeePassXC
* Automatically launch at startup
* Minimize window at startup
* DON'T minimize after unlockin

* Automatically save after every change
* Automatically reload the database when modified externally
* Backup file to somehwere local (not the SyncThing folder or any other cloud storage!)
* Use alternative saving method: Temporary file moved into place (needed for SyncThing!)

* Minimize instead of app exit
* Show a system tray icon
    * Hide window to system tray when minimized

#### 15.2.2 Security

* Lock database after 300 sec
* Clear search query after 5 min

#### 15.2.3 Browser integration

* Enable browser integration
    * Firefox
    * Chromium
    * Tor Browser
    * NOTE: you will have to change the firejail config for KeepassXC for each of these; check the default profile for how to do this


## References

* https://web.archive.org/web/20140220055801/http://crunchbang.org:80/forums/viewtopic.php?id=24722
* https://wiki.archlinux.org/title/Security
* https://web.archive.org/web/20210712001756/https://developer.ibm.com/technologies/linux/articles/l-harden-desktop/
* https://github.com/lfit/itpol/blob/master/linux-workstation-security.md
* https://www.reddit.com/r/Defcon/comments/ohl59z/bringing_your_personal_computing_devices_to_the/


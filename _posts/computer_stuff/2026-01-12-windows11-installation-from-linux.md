---
layout: post
title:  "How to make Windows 11 installation/recover media from linux"
summary: "Summarizing the process in case it helps someone else."
date:   
tags: [i use arch btw, computer stuff]
published: false
---

I helped my partner's much younger brother build a gaming PC last weekend and opted to install Windows 11 on it partially to allow him to easily mod games, partially because he already has a Steam Deck running linux, but mostly because he has no experience with Windows and he probably should learn before starting college/any sort of office job.

I downloaded the Windows 11 installation iso from Microsoft's website and used `dd` to copy it directly to a USB flash drive to use as the installation media. Trying to boot from this disk did load into a live Windows environment, however I got an error that said "Install driver to show hardware" and could not see any of the installed storage devices (an NVME drive in this case) despite that they showed up in the BIOS. After rabbit holing looking for what drivers I needed to fix this and ignoring all warnings that I must have created the installation media improperly (it booted! what could I have possibly done wrong?), I finally capitulated and acknowledged that there was an issue with how I create the install drive.

Windows and Linux iso files are different [1][1] and as such cannot be flashed to a thumb drive the same way. That's why Microsoft offers the Windows installation media creation tool, which unfortunately only runs on Windows (which is silly if you think about it; how can you swich *to* Windows if you don't already have it to create an install disk? Then again, who is switching from Mac/Linux to Windows?). Their tool will properly create a boot media using a given iso file.

Luckly, linux users have a tool that does the same thing: WoeUSB. It replicates the process that the Microsoft utility uses to take an iso and apply it to a USB drive. On Arch Linux, you can install it from the AUR[2][2] using yay:
    yay -S woeusb
And then run it like so:
    sudo woeusb --device /path/to/Win11_23H2_EnglishInternational_x64.iso /dev/sdX

This successfully created a working installation disk and we were able to proceed. We additionally used a Windows 11 Pro key purchased on the grey market [3][3] which I am happy to report actually worked and was not a scam, so I can endorse the linked site.

[1]: https://linuxiac.com/how-to-create-a-windows-bootable-usb-on-linux/
[2]: https://aur.archlinux.org/packages/woeusb
[3]: https://www.kinguin.net/category/110936/windows-11-professional-oem-key

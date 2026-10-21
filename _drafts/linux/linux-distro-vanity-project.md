---
layout: post
title:  "My GNU/Linux distro vanity project"
summary: "Because there aren't already enough indistinguishable flavors of Debian Linux floating around."
date:
last_updated:
tags: [linux, projects]
published: false
assetdir: 
---

At some point in the last year, I was made aware of "The Lunduke Journal", an online gossip ragsheet maintained by drama queen Bryan Lunduke that purports to be "the last bastion of truly independent Tech Journalism" (I guess 2600 saw its trademark get bought out by private equity without me hearing about it), under which banner Bryan announced his own Linux distro vanity project: the Lunduke Computer Operating System. Among other features, his operating system promises to be 100% Non-Woke (which I suspect means that it forces you to keep calling your primary git branch "master" instead of "main"), maintain a safe social distance from systemd, and avoid any and all memory-safe reimplementations of aging tools in the Rust programming language because Rust is for lesbians. He posts occasional videos about it on his facebook page, the layout of which look like something you'd see on InfoWars: zoomed in video of his graying Gen-X face in the corner cropped to hide his messy home, blown-up screenshare of whatever latest small-potatoes tech world melodrama he wants to show you, all on top of a sensationalist chyron scrolling along the bottom of the video. It would actually be a bit of an intriguing project if it weren't so publicly masturbatory; not that there's a dearth of anti-systemd distros floating around the web ([antiX](https://antixlinux.com/), one of the more popular ones, ironically is the diametric opposite of Lunduke's professed politics), but it would be useful as an A/B comparison between a Linux derivative that proudly proclaims to remain as old-and-busted as possible versus the Arch/Debian/Fedora new-hotness end of the spectrum in terms of platform support, memory footprint, stability, package compatability, etc. It also made me realize that if a schmuck like Bryan Lunduke can make a vanity project Linux distro, so can I.

<!--excerpt-->

## Why

To put it simply: I want a fire-and-forget solution for spinning up a fresh GNU/Linux desktop environment that doesn't require internet connectivity to deploy. Yes, you can use a post-install script to customize Debian however you like, but that requires you have a stable and trustworthy internet connection. What if you're at your grandparents' remote Canadian home with dial-up speed internet and they are running a Windows 8.1 laptop in desperate need of wiping and OS replacement, along with pre-packages office suite? That's where an offline installer loaded with the packages you care about comes in handy.

I want something that is reasonably, not onerously, secure off-the-shelf without superfluous security tooling that InfoSec practioners need but aren't desirable in an everyday use desktop system. I'm not trying to recreate Qubes or Kali here; those both serve their purposes very well and don't need to be iterated upon. This distro needs to be something that the slightly-above-average operator can use and feel comfortably protected against corporate online tracking, misconfigured wifi networks, script kiddies looking for vulnerable machines at their local community college, or misbehaving applications.

I want something that can be feasibly deployed on older or anemic hardware. Have an old Intel Chromebook laying around that is otherwise going to become e-waste? It should be easy to turn it into a working, general purpose netbook with a fire-and-forget OS installer.

## What

For years, I have used Crunchbang and its successor Crunchbang++ as my go-to quick-and-dirty desktop OS. It's about 85% of the way to a system I would like to use, but is missing several key pieces of software and uses some configurations that I think are not ideal; I am tempted to even host a repository that includes software that I think belongs in the Debian stable repos. I will be drawing heavily from CB++ for inspiration as to which packages should be included by default.

For this project, I would use Debian minimal as a base. There is lots of whining on the internet about how out-of-date Debian packages are, but there's also lots of whining about how unstable Arch can be and how big Fedora's memory footprint is. Arch also doesn't have any security features in a fresh install, and Fedora uses the more complicated SELinux rather than apparmor for mandatory access control which Debian has enabled by default. If this is supposed to be a secure, stable operating system that you can deploy in an igloo with no connection to the internet, I want it to use packages that have been vetted as stable and ready to ship, and the Debian team does exactly that.

The kernel needs to include drivers for older hardware in case this is deployed on older systems, however I am intentionally only going to focus on x86_84 architectures to avoid overdiversifying supported systems. This means that the Zen kernel is out of the question, as it is missing support for even some more recent hardware off-the-shelf.

### What I'd keep

Anything not explicitly earmarked for removal, replacement, or upgrade should be kept. Notably, the xfce desktop environment packages that CB++ uses are a great base to build on top of with i3 and tint2 as the actual window manager.

### What I'd remove

* lxdm
  - This distro is targeting neither powerusers nor beginners but operators somewhere in between, and I expect them to be competent enough to log in using a tty and run `startx` when they need a graphical environment

### What I'd replace

* i3 instead of openbox:
  - openbox is a bit bloated and tiling window managers require less manual rearranging of windows on smaller laptop screens
  - I would, however, keep tint2 to use instead of the i3bar, configured almost exactly the way it is in CB++
* rxvt-unicode instead of terminator:
  - terminator is great for graphical customization and easy screen splitting, but urxvt is lighter, more customizable, and doesn't need built-in screen splitting if using i3
* sublime vs the other graphical text editors:
  - I almost always just use vim in a terminal, but some users aren't comfortable with that
  - Sublime is available on Linux and Windows, so it would make the pivot more of a lateral move

### What I'd add

Some of these may require the inclusion of developer repositories (e.g. Mozilla) that supercede the Debian stable repo. Others I have had to build from source, which inclines me towards maintaining a repo specific to this project to host pre-compiled binaries for those software that I want to see included (similarly to the Arch Users Repository).

* Current builds of Mozilla software:
  - Firefox
    * Shut the fuck up about Brave. Just shut the fuck up. I refuse to cede the internet to the Chromium engine.
  - Thunderbird
* firejail
  - firejail's exclusion from the Debian stable repos is a huge oversight since there isn't any sandboxing included by default (like there is in atomic Fedora)
  - combined with apparmor, this already is a more secure system than most other off-the-shelf Linux distros and avoids the more complicated SELinux bundled with Fedora
* gufw 
* opensnitch
* USBguard
* KeePassXC
* Yubico Authenticator
  - another piece of software that typically has to be built from source
  - enables ready and easy use of Yubico hardware authentication tokens
* Mullvad VPN
  - while you certainly can set up Mullvad to work using OpenVPN or Wireguard, their GUI has additional features that are otherwise complicated to configure
* Protonmail Bridge
  - ideally I want to automate its deployment to work with Thunderbird
  - also I hope to automate the creation, sharing, and storage of PGP keys for email on the operator's behalf
* zsh
  - I would leave bash as the default in the interest of memory conscientiousness, but zsh should be installed at the outset
* vundle, vimwiki, and the calendar plugin
  - these are phenomenal vim plugins that make brain-dumping so much easier
* VirtualBox, extensions, and the Oracle repo
  - Yes, yes, Oracle is evil but VirtualBox is the most widely used free-as-in-free-beer virtual machine manager, and it enables operators to share VM files and virtual disks with other VirtualBox users more easily
* Signal messenger

### Conigurations I'd make

* disable bluetooth and firewire kernel modules
* reasonably strict default firewall rules
* custom firejail profiles for pre-installed applications




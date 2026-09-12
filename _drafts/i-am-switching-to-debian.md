---
layout: post
title:  "I'm switching to Debian from Arch"
summary: "Having to maintain a rolling release distro while moving was too much of a hassle."
date:   
last_updated:
tags: [linux]
published: false
assetdir: 
---
I have used Arch Linux since roughly ~2017 (maybe earlier, if you count Arch ARM). It was overall trim, stable, and gave me a great amount of control over my machine. The main drawback is that if you don't update your system regularly (bare minimum monthly basis), you run the risk of catastrophic breakages due to most packages being severely behind. This was not an issue for me...until it was, namely over the past two months while I moved homes and didn't have the time or access to each of my computers such that I could update everything in a timely manner. I don't want to deal with catastrophic breakages on mission-critical machines, so I am going to convert all my machines to Debian (or rather, [Crunchbang++][cbpp]) very soon...and then maybe try FreeBSD out after that.
<!--excerpt-->

## Why Debian

One thing I noticed during my Debian install: a lot of security features that I have to manually enable on Arch were already pre-enabled on Debian, such as apparmor. 

## Issues I've run into so far

Lots of packages that are in the core Arch repositories are not in Debians; firejail is an example of this, but installing it from git instead was not difficult and easily scriptable.

I also experienced the FOMO of not having up-to-date packages that you get from a bleeding edge distro like Arch, especially using a distro like Debian whose package adoption lags by design to preserve stability. This was most noticeable with Mozilla software, specifically Firefox and Thunderbird, but again it wasn't difficult to set up Mozilla's repositories to use with apt.

## Process

Before I made the abrupt decision to move, I was planning to attend DEFCON and had earmarked my Lenovo T480 as the designated DC laptop. It would need an operating system that might only see use once or twice a year, so Arch was already not a contender. I went with my second-favorite (former favorite) desktop Linux distro: Crunchbang++, which is little more than a minimalist debian skin with some commonly used packages pre-installed.

### Hardening the system

, and it needed to be as hardened as is reasonable. What is reasonable in this context? The truly paranoid would suggest just not bringing any wireless electronics to DEFCON, but by all accounts that's overkill. The machine just needed to be impervious to insecure networks and drive-by Bad USB attacks, with typical preventative, detective, and corrective controls installed. 

I am working on [a guide covering how to harden Debian][hardening-guide] which I will post once completed.

### Adding Mozilla repositories

### Adding Yubikey Authenticator

### Installing firejail

## Distro Euphoria

As I was getting the system set up for DC, I had a realization: this setup isn't actually half bad, and also I really like the T480 as a do-almost-everything computer. I don't have to worry about packages falling out of currency. The system is well-hardened. It docks with my super-ultrawide monitor/kvm easily enough. And the keyboard is better than my StarLite, Chromebooks, or old Gigabyte laptop. 

So now my con laptop is my daily driver.

[cbpp]: 

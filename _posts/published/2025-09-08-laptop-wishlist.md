---
layout: post
title:  "My wishlist for the perfect laptop"
summary: "If you build it, I will come"
tags: [hardware]
published: true
---

I got a bug up my ass to figure out if it was possible to [disable the Intel Management Engine][disable-Ime] on any of the devices I own (spoiler alert: probably not and if so it's not a trivial task), and that led me to stumbling upon [Leah Rowe][leah-rowe]'s [Minifree][minifree] project and her refurbed [Thinkpad T480s][libreboot-t480] that she sells with the Intel ME successfully disabled. It's a very attractive proposition and probably what I am going to recommend to people -- especially the more privacy conscious -- when they ask me what laptop they should get. But it's still missing some things that I want to see in a laptop.

I think the best laptop I've ever owned was a [MacBook Pro 5,5][mbp] that I used my final year of high school and most of college until the motherboard died, the final entry in a series of lemons that I got from Apple between 2007 and 2009. It survived a fall out of my bag onto the asphalt without a protective case, striking the top corner and permanently denting it on the screen. For the following three years, I triple-booted OS X, Windows, and Linux on that laptop with few meaningful issues. I am also pretty sure I had a whole Bitcoin stored on that machine but now the wallet files are lost to time and me absent-mindedly scrubbing the drive and wrongly assuming I had backed up the wallet to Dropbox.

I replaced that laptop with a Samsung Chromebook that used an ARM chip. I found that so long as I had a desktop computer or access to a server to do higher-performance computing tasks, a Chromebook or netbook was perfectly sufficient for my portable computing. I continued to use that Chromebook for the next couple of years into grad school, often just using Chrome Remote Desktop to log into my computer back home while I was sitting in a lecture. 

Eventually, I installed the ARM fork of Arch linux onto an SD card once ChromeOS became too bloated for the computer's meager hardware, and after a negative experience asking for help with ArchARM on the regular Arch forums (they told me to fuck off to the forum for my shitty fork, and honestly they were right to do so), I replaced the Samsung Chromebook with an [Acer c720 Chromebook][c720] that used the good 'ole Intel x86_64 instruction/chipset. This enabled me to install proper Arch Linux that exclusively supports x86 processors. Roughly eight years later, that Acer Chromebook that I paid ~$30 for on eBay has outlasted the $1,000+ MacBook that didn't even last four years. It's my primary portable laptop, my "daily driver", and the computer that I'm typing this on. I found that laptop to be so cost effective that I purchsed two more to use as travel laptops or to cannibalize for parts should the need arise; unfortunately those two were the 2Gb RAM versions with shittier trackpads that came after the c720-2844 which has 4Gb of RAM and a three-button trackpad, but they're good enough as spares.

One other laptop that I briefly considered as a project computer was the [Sony Vaio P][vaio-p] "lifestyle" netbook that at the time of release was pitched as the missing link between phones/PDAs and laptops. It has somewhat of a cult following in the [UMPC][umpc] community, however its paltry 2Gb of non-upgradeable ram and a fragile screen on top of a $200+ pricetag in 2025 made it an impractical purchase as a functional laptop.

As far as modern laptops go, I'm intrigued by the [Framework][framework] laptop concept that enables users to upgrade and repair their laptop over time as things break or as their needs change. The major drawbacks I see with these is that they don't offer a 11" or smaller laptop, and their smallest laptop comes with a plastic shell. They are also pretty expensive compare to all-in-one laptops with comparable specs.

# The Perfect Laptop

Each of the above laptops offered something desirable that I wish some manufacturer would combine into a single, perfect laptop. Included is a list of features both functional and aesthetic that should any manufacturer combine into a single device I would purchase and make into my main "professional", non-gaming computer.

## Aluminum shell

Any laptop with a plastic shell, particularly larger ones (like my 17" Gigabye laptop), will eventually see the chassis fall apart. This will usually happen at the hinges, however I've seen it happen on part of the housing where the user's wrists rest (which is obviously profoundly uncomfortable).

Aluminum is more durable and arguably more impact-resistant than plastic. It also doesn't terrify me as much as carbon fiber which, while lighter and theoreticaly more durable than aluminum, can cause very nasty splinters in the rare instance that it does break.

## Etched glass three-button trackpad

My old MacBook Pro had a perfect trackpad that I have never seen any other brand besides Apple produce. I don't undestand what is so damn hard about making a trackpad that doesn't suck, but it's wild that only one company has cracked the code here. The only thing that could improve that design is if they made it three-button instead of two-button.

## Replaceable Wi-Fi 6 network controller

The Libreboot T480 is modified with a Wi-Fi 6 network controller, which I think should be a bare minimum feature on any modern laptop. However, Wi-Fi 7 already exists and will eventually become the new standard, so being able to easily (or perhaps even externally) hot swap the wireless interface in a similar fashion to how Framework allows buyers to hot swap ports on their laptop would be useful. Having multiple Wi-Fi interfaces is useful for security professionals, particularly penetration testers, but to my knowledge nobody has implemented this ouside of external USB dongles.

## x86_64 processor with Intel ME completely disabled

ARM and [RISC V][risc-v] just don't have the vast support that x86 enjoys. Perhaps one day they will see wider adoption, but until then a number of programs that I use professionally only run on x86.

The obvious issue here is that x86 makes the laptop vulnerable to the Intel Management Engine's theoretical backdoor (still has yet to be seen in the wild, but if the NSA is disabling it on their machines you know it's a legitimate concern). Disabling the ME is getting more difficult with each generation of Intel chipsets, and only a few have documentation for using [me_cleaner][me_cleaner] to disable it. Selling a laptop like those that System 76 or Purism do that have it disabled at the hardware level would be idea.

## 10" form factor 1920x1200 screen

The point of a laptop is to be portable. My 11" Chromebook is, so far, the most easy-to-port computer that I have ever owned, and even still I find that I wish that it was just a bit smaller so that it would fit into my mini Kavu sling bag. 13" to 14" seems to be the average laptop size, and I dont' know of any well known, reputable manufacturer making netbooks anymore, which is a shame.

Typing on a phone absolutely sucks, especially if you have osteoarthritis. I literally use my laptop to respond to SMS and Signal messages because typing on a phone is so uncomfortable (it's probably no surprise that I don't use SWYPE because it isn't exactly the [most private keyboard available on Android][futo-keyboard]). External physical phone keyboards are too small to type like you would on a computer keyboard, so I often just wait until I get back to my computer to respond to messages or emails if I'm outside the home without my laptop. If my computer was just a tad smaller, I could just take it everywhere.

When the screen is that small however, the battery shrinks with it. Cramming a 1440p monitor into a laptop like that and a GPU that can support it will leave you with extremely small text and a pathetic battery life.Meanwhile, 1920x1200 is more conservative but still gives you just a little bit more vertical real estate for reading and typing than a typical 16:9 resolution.

## Low-profile mechanical keys

Membrane keyboards will eventually wear out, and those on cheap netbooks, chromebooks, and UMPCs are especially vulnerable because they were made to be exactly that: cheap. I try to be as gentle as possible when typing on my Chromebook so that I can get another 8 years out of this thing. But if someone came alont and developed a low-profile mechanical switch and key cap design that was meant for laptops, that would be a godsend.

Yes, this would lead to the laptop being thicker than the almost paper-thin laptop designs being sold now. But I think the juice is worth the squeeze here.

## Swapable ports

Framework had the right idea: let users hot swap their ports. I think a base config would look like:

* USB-C
* USB 3.1
* Ethernet
* 3.5mm audio jack

but I'm one of those old heads that still uses Ethernet and analog headphones. As mentioned above, a removable Wi-Fi dongle that fit into the port slots would also be really cool, especially if it had a visible antenna.

## Upgradealbe RAM

I think what I really mean is Bring Your Own RAM. I think most people would do either 16Gb or 64Gb, depending on how bad they are about closing browser tabs. Miraculously, I am able to do 80% of my computer stuff with just 4Gb of RAM and a 4Gb swap partition. But 16Gb would be nice to have instead, if only so I can let ClamAV and SyncThing run as background daemons all the time. 

## Libreboot or GNUboot

Just let me go directly to grub.

# Wouldn't it be nice

No single laptop, to my knowledge, has ever incoporated all these features. The few that remove or disable Intel ME are focused on building 13+" laptops with plastic shells. The ones with aluminum shells and nice trackpads all use Apple's goofy proprietary chipset. The Framework laptops seem to generally get mixed reviews, but I think if any company would be able to pull off the concept I am proposing here it'd be Framework. Even if it ends up being ~$2,000, I think it would be a better investment than an even more expensive MacBook.

Until then, I think I'll stick with my $30 Chromebook. At least I know I got well more than I paid for.

[disable-Ime]: https://hackaday.com/2023/04/12/disabling-intels-backdoors-on-modern-laptops/
[leah-rowe]: https://vimuser.org/
[minifree]: https://minifree.org/
[libreboot-t480]: https://minifree.org/product/libreboot-t480.html
[mbp]: https://support.apple.com/en-us/112474
[c720]: https://wiki.archlinux.org/title/Acer_C720_Chromebook
[vaio-p]: https://www.theverge.com/2016/10/30/13473970/sony-vaio-p-2016-review-tokyo-thrift 
[umpc]: https://www.umpcportal.com/
[framework]: https://frame.work/
[me_cleaner]: https://github.com/corna/me_cleaner/wiki/How-to-apply-me_cleaner
[futo-keyboard]: https://keyboard.futo.org/



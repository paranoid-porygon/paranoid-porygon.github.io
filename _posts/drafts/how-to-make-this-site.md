---
layout: post
title:  "HowTo: this site"
summary: "Overview of the tech stack needed to make this site"
date:   
tags: [jekyll, META]
published: false
---

Primarily to use as my own documentation, but also for the sake of sharing with others looking to make a similar site, I want to chronicle the steps I took and the resources I referred to in the construction of [dubious-disc.blog](https://dubious-disc.blog).

In short: you will need a domain purchased through a reputable registrar, a GitHub account and familiarity with the platform and with git, a linux machine, and a little experience with HTML, CSS, YAML, and Liquid scripting/templating languages (if you've ever made custom templates for Xanga, Tumblr, or your Neopets' pages, that should be sufficient!).

<!--excerpt-->

# TODO

## Migrate off of GitHub

Given Microsoft's [heavy investment into LLMs trained on user data](), I plan to move my site off of the Microsoft-owned GitHub to a self- or VPS-hosted platform instead. 

GitHub was convenient and there was plenty of documentation about how to get a Jekyll site up and running quickly using the platform, however the less control you have over the infrastructure, the less private the platform is. Additionally, GitHub/Microsoft have the ability to censor my site if they got that kind of bug up their ass. Therefore I plan to pivot to hosting this page on a Virtual Private Server (VPS).

A VPS might not be *as private* as self-hosting the site on a server that I physiclaly own, but doing the latter would require exposing my home server and network to the internet in ways that are beyond my level of comfort. A single-purpose VPS becoming compromised poses significantly less risk than my Proxmox cluster getting compromised, exposing my storage, network, etc.

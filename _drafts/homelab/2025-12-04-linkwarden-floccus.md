---
layout: post
title:  "Homelab: using Linkwarden and Floccus to store and sync my browser bookmarks"
summary: "An easier and more extensible solution than hosting your own Firefox sync and account servers."
date: 2025-12-01  
tags: [homelab, dealgorithming]
published: false
---


## Firefox setup

* install floccus extension
* run through setup
## create profile for both bookmarks bar and other bookmarks

* specify LinkWarden account
* paste IP address or internal URL AND PORT NUMBER to your LinkWarden app on your TrueNAS box
* paste username used to access LinkWarden Web GUI
* in the web GUI, create an access token for the web browser you are setting up (you'll do this for each browser and machine you add)
    - click on your username in the lower left corner, and click settings in the popup menu
    - on the sidebar, click `Access Tokens`
    - create a new one for the given browser
    - copy the generated token and paste it somewhere to use for each bookmark folder you want to sync; you don't want to have to make multiple tokens for a single browser
    - go back to the Floccus setup and paste it in the respective field
* when selecting folders you want to sync:
    - the `Server Target` is the exact name of the existing LinkWarden collection that you want to sync with either your local Firefox bookmarks bar or menu; if pulling down an existing collection, the name must match exactly
    - `Local Target` is which local Firefox bookmarks folder you want to sync, either the bar or menu

Once finished, if this is the first time setting up a Floccus profile, click on the extension button on your menu bar and hit "upload once". If you already have bookmarks stored on LinkWarden, instead click "download once". Going forward, your bookmarks *should* automatically synchronize every 15 minutes.


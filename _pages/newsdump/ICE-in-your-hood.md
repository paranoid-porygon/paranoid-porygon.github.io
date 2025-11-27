---
layout: default
title:  "ICE"
permalink: /newsdump/ICE/
published: true
---

# ICE is the American Stasi

If you're a liberal, libertarian, or leftist, chances are the prospect of masked federal goons abducting you or your neighbors without charge under the auspices of "preserving national security" or "fighting crime" has kept you up at night since the Bush Jr. years. Civil rights activists have been loudly warning that the emergency powers given to the executive branch in the aftermath of 9/11 will at some point be utilized in the most Kafkaesque ways imaginable. Well those people were right and we are finally seeing what happens when you cede too much power to the executive and you elect someone who doesn't give a rat's ass about norms, the law, or basic decency and civility: ICE has become America's Stasi and is black-bagging permanent residents, visa-holders, and even citizens without due process or informing them why they are being "detained". 

This post will become a dumping ground for news about ICE's activities and resources advising on how to frustrate their efforts. I will update it periodically.

## How to Fight Them

* [What to do when you see ICE in your neighborhood](https://theintercept.com/2025/07/12/ice-neighborhood-watch-la/)

## ICE's activities

<ul>
{% assign news = site.newsdump | sort: 'date' | reverse %}
{% for post in news %}
{% if post.tags contains 'ICE' %}
<li> <a href="{{ post.dest }}" target="_blank" rel="noopener noreferrer">{{ post.date | date_to_string }}</a> - {{ post.title }}</li>
{% endif %}
{% endfor %}
</ul>
## Related Court rulings

* [Justice Kavanaugh makes it completely clear that the Court's decision to legalize racial profiling is foundationally racist with no basis in legal precedent or fact-based argument](https://www.publicnotice.co/p/kavanaugh-ice-racial-profiling)


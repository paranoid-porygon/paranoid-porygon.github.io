---
blinkies:
  - www:
    - "the www project"
    - "www"
    - "https://info.cern.ch/hypertext/WWW/TheProject.html"
---

{% for blinky in page.blinkies %}
[![{{ blinky[0] }}](/assets/badges/{{ blinky[1] }}.gif)]({{ blinky[2] }})
{% endfor %}


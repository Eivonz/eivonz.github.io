---
layout: default

---

# All the things...
But mostly some of the things



## PHP Malware analysis

{% assign subfolder = site.pages | where_exp: "item" , "item.path contains 'php'"%}
{% for item in subfolder %}
<a {% if item.url == page.url %}class="active"{% endif %} href="{{ site.baseurl }}{{ item.url }}">{{ item.title }}</a>
{% endfor %}



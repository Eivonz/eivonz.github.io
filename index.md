---
layout: default
---

<!--
# All the things...
But mostly some of the things

## PHP Malware analysis

{% assign subfolder = site.pages | where_exp: "item" , "item.path contains 'php'"%}
{% for item in subfolder %}
<a {% if item.url == page.url %}class="active"{% endif %} href="{{ site.baseurl }}{{ item.url }}">{{ item.title }}</a>
{% endfor %}
-->

Various stuff related to development, reverse engineenering and related.

## Latest Posts
<div>&nbsp;</div>

<style>list-style: none;</style>

{% for post in site.posts %}

<hr/>

<p>{{ post.date | date: "%b %-d, %Y" }}</p>
<h2><a class="post-link" href="{{ post.url | relative_url }}" title="{{ post.title }}">{{ post.title | escape }}</a></h2>

{{ post.excerpt | markdownify | truncatewords: 30 }}

{% endfor %}

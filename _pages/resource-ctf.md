---
title:  "CTF Writeups"
layout: archive
permalink: /ctf/
author_profile: true
comments: false
resource: true
categories: [ctf]
---

This page lists all the CTF writeups on this blog.

<ul>
{% for post in site.posts %}
	{% if post.categories contains 'ctf' %}
  	{% unless post.next %}
    	<font color="#778899"><h2>{{ post.date | date: '%Y %b' }}</h2></font>
  	{% else %}
   	 {% capture year %}{{ post.date | date: '%Y %b' }}{% endcapture %}
    	{% capture nyear %}{{ post.next.date | date: '%Y %b' }}{% endcapture %}
    	{% if year != nyear %}
      	<font color="#778899"><h2>{{ post.date | date: '%Y %b' }}</h2></font>
    	{% endif %}
  	{% endunless %}
 		{% include archive-single.html %}
{% endif %}
{% endfor %}
</ul>
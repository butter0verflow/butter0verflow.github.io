---
title:  "OSCP Resources"
layout: archive
permalink: /oscp/
author_profile: true
comments: false
resource: true
categories: [oscp]
---

This page lists all the OSCP related posts on this blog.

<ul>
{% for post in site.posts %}
	{% if post.categories contains 'oscp' %}
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
---
title:  "Misc"
layout: archive
permalink: /misc/
resource: true
categories: [misc]
---

This page lists all the uncategorized posts on this blog.

<ul>
{% for post in site.posts %}
	{% if post.categories contains 'misc' %}
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
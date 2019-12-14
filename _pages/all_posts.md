---
title:  "All Posts"
layout: archive
permalink: /all_posts/
author_profile: true
comments: false
---

List of all posts on this blog:

#<ul>
#  {% for post in site.posts %}
#    {% unless post.next %}
#      <font color="#778899"><h2>{{ post.date | date: '%Y %b' }}</h2></font>
#    {% else %}
#      {% capture year %}{{ post.date | date: '%Y %b' }}{% endcapture %}
#      {% capture nyear %}{{ post.next.date | date: '%Y %b' }}{% endcapture %}
#      {% if year != nyear %}
#        <font color="#778899"><h2>{{ post.date | date: '%Y %b' }}</h2></font>
#      {% endif %}

#    {% endunless %}
#   {% include archive-single.html %}
#  {% endfor %}
#</ul>
<ul>
	{% for p in site.pages %}
   		{% if p.categories contains page.category %}
     		* [{{ p.title }}]({{ p.url | absolute_url }})
        		<small>{{ p.excerpt }}</small>
   		{% endif %}
	{% endfor %}
</ul>

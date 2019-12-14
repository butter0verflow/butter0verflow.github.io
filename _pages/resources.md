---
title:  "Resources"
layout: archive
permalink: /resources/
---

This page lists all the sections on this blog.
{% for cat in site.category-list %}
  {% for page in site.pages %}
    {% if page.resource == true %}
      {% for pc in page.categories %}
        {% if pc == cat %}
          <li><a href="{{ page.url }}">{{ page.title }}</a></li>
        {% endif %}   <!-- cat-match-p -->
      {% endfor %}  <!-- page-category -->
    {% endif %}   <!-- resource-p -->
  {% endfor %}  <!-- page -->
{% endfor %}  <!-- cat -->

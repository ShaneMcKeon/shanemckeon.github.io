---
layout: archive
title: " "
permalink: /publications/
author_profile: true
redirect_from: 
  - /wordpress/academic-papers/
---

{% include base_path %}

 <ol>{% for post in site.publications reversed %}
    {% include archive-single.html %}
  {% endfor %}</ol>

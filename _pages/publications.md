---
layout: archive
title: " "
permalink: /publications/
author_profile: true
redirect_from: 
  - /wordpress/academic-papers/
---

You can also find my articles on <u><a href="https://scholar.google.com/citations?user=NVYZQx4AAAAJ&hl=en">my Google Scholar profile</a>.</u>


{% include base_path %}

 <ol>{% for post in site.publications reversed %}
    {% include archive-single.html %}
  {% endfor %}</ol>

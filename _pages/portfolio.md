---
layout: archive
title: "Projects & Code Documentation"
permalink: /portfolio/
author_profile: true
---


{% include base_path %}

{% assign sorted_projects = site.portfolio | sort: "date" | reverse %}
{% for project in sorted_projects %}
  {% include archive-single.html %}
{% endfor %}

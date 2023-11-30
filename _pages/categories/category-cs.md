---
title: "CS"
layout: archive
permalink: /categories/cs/
author_profile: true
sidebar:
  nav: "categories"
---

{% assign posts = site.categories.Cs %}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
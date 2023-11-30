---
title: "Programmers"
layout: archive
permalink: /categories/programmers/
author_profile: true
sidebar:
  nav: "categories"
---

{% assign posts = site.categories.Programmers %}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
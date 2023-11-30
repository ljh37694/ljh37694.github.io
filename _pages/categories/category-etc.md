---
title: "ETC"
layout: archive
permalink: /categories/etc
author_profile: true
sidebar:
  nav: "categories"
---

{% assign posts = site.categories.Etc %}
{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
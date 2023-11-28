---
title: "프로그래밍 언어"
layout: archive
permalink: categories/language
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.language %}
{% for post in posts %} {% include archive_single.html type=page.entries_layout %} {% endfor %}
---
layout: page
title: About
permalink: /about/
header: true
---

A basic daily notebook, inspired by [David MacIver's practice]({{site.baseurl}}{% post_url 2020-06-15-first %}).

You may also be interested in my [main blog](https://www.louispotok.com).

Besides daily posts, this site contains the following pages:
{% for page in site.pages %}
  {%- if page.visible == true -%}
   <li><a href="{{site.baseurl}}{{ page.url }}">{{ page.title }}</a></li>
 {%- endif -%}
{% endfor %}



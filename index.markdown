---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: ""
---

{% for page in site.pages %}
  {%- if page.visible == true -%}
   <li><a href="{{ page.url }}">{{ page.title }}</a></li>
 {%- endif -%}
{% endfor %}



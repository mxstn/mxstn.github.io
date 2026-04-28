---
permalink: /photography
layout: page
title: Photography
---

<div class="gallery">
{% for file in site.static_files %}
  {% if file.path contains '/assets/img/imp/' %}
    {% if file.extname == '.jpg' or file.extname == '.png' %}
      <img src="{{ file.path | relative_url }}" alt="" width="100%" oncontextmenu="return false;"/>
    {% endif %}
  {% endif %}
{% endfor %}
</div>

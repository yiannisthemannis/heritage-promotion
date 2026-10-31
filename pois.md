---
layout: page
title: Ελληνικά Πιάτα
permalink: /pois/

---

<div class="grid-container">
  {% for p in site.pois %}
    <div class="card">
      <h3><a href="{{ p.url | relative_url }}">{{ p.title }}</a></h3>
    </div>
  {% endfor %}
</div>

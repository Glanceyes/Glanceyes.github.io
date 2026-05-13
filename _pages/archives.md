---
title: Archives
subtitle: A curated repository for my study notes, insights, and write-ups
layout: page
permalink: /archives
featured_image: /images/archives/banner_image_new.jpg
---

<div class="archive-grid">
  {% assign sorted_archives = site.archives | sort: "order" %}
  {% for entry in sorted_archives %}
  <a class="archive-card" href="{{ entry.url | relative_url }}">
    {% if entry.thumb %}<img class="archive-card-thumb" src="{{ entry.thumb | relative_url }}" alt="{{ entry.title }}">{% endif %}
    <div class="archive-card-body">
      {% if entry.category %}<span class="archive-card-category">{{ entry.category }}</span>{% endif %}
      <div class="archive-card-title">{{ entry.title }}</div>
      {% if entry.topics %}<div class="archive-card-topics">{{ entry.topics }}</div>{% endif %}
      {% if entry.excerpt_short %}<div class="archive-card-excerpt">{{ entry.excerpt_short }}</div>{% endif %}
    </div>
  </a>
  {% endfor %}
</div>

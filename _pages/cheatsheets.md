---
layout: page
title: cheatsheets
permalink: /cheatsheets/
description: A growing collection of trick-and-treating
nav: true
nav_order: 3
display_categories: [tools]
horizontal: true
---

<!-- pages/cheatsheets.md -->
<div class="projects">
{%- if site.enable_cheatsheet_categories and page.display_categories %}
  
  <!-- Display categorized projects -->
  {%- for category in page.display_categories %}
  <h2 class="category">{{ category }}</h2>
  {%- assign categorized_cheatsheets = site.cheatsheets | where: "category", category -%}
  {%- assign sorted_cheatsheets = categorized_cheatsheets | sort: "importance" %}
  <!-- Generate cards for each project -->
  
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for cheatsheet in sorted_cheatsheets -%}
      {% include cheatsheets_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  
  {%- else -%}
  <div class="grid">
    {%- for cheatsheet in sorted_cheatsheets -%}
      {% include cheatsheets.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
  {% endfor %}

{%- else -%}
<!-- Display cheatsheets without categories -->
  {%- assign sorted_cheatsheets = site.cheatsheets | sort: "importance" -%}
  <!-- Generate cards for each cheatsheet -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for cheatsheet in sorted_cheatsheets -%}
      {% include cheatsheets_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for cheatsheet in sorted_cheatsheets -%}
      {% include cheatsheets.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
{%- endif -%}
</div>

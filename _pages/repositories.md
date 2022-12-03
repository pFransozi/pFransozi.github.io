---
layout: page
permalink: /repositories/
title: repositories
description: My github user and some starred repositories.
nav: true
nav_order: 3
---

## GitHub My User

{% if site.data.repositories.github_my_user %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_my_user %}
    {% include repository/repo_user.html username=user %}
  {% endfor %}
</div>
{% endif %}

---

## Github Other Users

{% if site.data.repositories.github_other_users %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for user in site.data.repositories.github_other_users %}
    {% include repository/repo_user.html username=user %}
  {% endfor %}
</div>
{% endif %}

## GitHub Repositories: Programming

{% if site.data.repositories.github_repos_programming %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos_programming %}
    {% include repository/repo.html repository=repo %}
  {% endfor %}
</div>
{% endif %}

---

## GitHub Repositories: Cybersecurity

{% if site.data.repositories.github_repos_cybersecurity %}
<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos_cybersecurity %}
    {% include repository/repo.html repository=repo %}
  {% endfor %}
</div>
{% endif %}

---
layout: page
title: Team
permalink: /team/
description:
nav: true
---

<div class="projects grid">

  {% assign sorted_member = site.team | sort: "importance" %}
  {% for member in sorted_member %}
  <div class="grid-item">
    {% if member.redirect %}
    <a href="{{ member.redirect }}" target="_blank">
    {% else %}
    <a href="{{ member.url | relative_url }}">
    {% endif %}
      <div class="card hoverable">
        {% if member.img %}
        <img src="{{ member.img | relative_url }}" alt="member thumbnail">
        {% endif %}
        <div class="card-body">
          <h2 class="card-title text-lowercase">{{ member.title }}</h2>
          <p class="card-text">{{ member.description }}</p>
          <div class="row ml-1 mr-1 p-0">
            {% if member.github %}
            <div class="github-icon">
              <div class="icon" data-toggle="tooltip" title="Code Repository">
                <a href="{{ member.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
              </div>
              {% if member.github_stars %}
              <span class="stars" data-toggle="tooltip" title="GitHub Stars">
                <i class="fas fa-star"></i>
                <span id="{{ member.github_stars }}-stars"></span>
              </span>
              {% endif %}
            </div>
            {% endif %}
          </div>
        </div>
      </div>
    </a>
  </div>
{% endfor %}

</div>

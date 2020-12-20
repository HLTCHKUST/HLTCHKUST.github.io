---
layout: page
title:
short_title: Team
permalink: /team/
description:
nav: true
---

<h1 style="margin-top: 5rem; margin-bottom: 1rem;">Current Students</h1>
<div class="team grid">

{% assign sorted_member = site.team | sort: "importance" %}
{% for member in sorted_member %}
  {% if member.status == "current_student" %}
  <div class="grid-item">
    <!-- {% if member.redirect %}
    <a href="{{ member.redirect }}" target="_blank">
    {% else %}
    <a href="{{ member.url | relative_url }}">
    {% endif %} -->
    {% if member.personal_url %}
    <a href="{{ member.personal_url}}" target="_blank">
    {% endif %}
      <div class="card hoverable">
        {% if member.img %}
        <img src="{{ member.img | relative_url }}" alt="member thumbnail">
        {% endif %}
        <div class="card-body">
          <h3 class="card-title">{{ member.title }}</h3>
          <p class="card-text">{{ member.description }}</p>
          <div class="row ml-1 mr-1 p-0">
            {% if member.email %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.email }}" target="_blank"><i class="far fa-envelope gh-icon"></i></a>
            </div>
            {% endif %}
            {% if member.github %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
            </div>
            {% endif %}
            {% if member.linkedin %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.linkedin }}" target="_blank"><i class="fab fa-linkedin gh-icon"></i></a>
            </div>
            {% endif %}
            {% if member.scholar %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.scholar }}" target="_blank"><i class="fas fa-graduation-cap gh-icon"></i></a>
            </div>
            {% endif %}
          </div>
        </div>
      </div>
    {% if member.personal_url %}
    </a>
    {% endif %}
  </div>
  {% endif %}
{% endfor %}

</div>

<h1 style="margin-top: 5rem; margin-bottom: 1rem;">Alumnus</h1>
<div class="team grid">

{% assign sorted_member = site.team | sort: "importance" %}
{% for member in sorted_member %}
  {% if member.status == "alumni" %}
  <div class="grid-item">
    <!-- {% if member.redirect %}
    <a href="{{ member.redirect }}" target="_blank">
    {% else %}
    <a href="{{ member.url | relative_url }}">
    {% endif %} -->
    {% if member.personal_url %}
    <a href="{{ member.personal_url}}" target="_blank">
    {% endif %}
      <div class="card hoverable">
        {% if member.img %}
        <img src="{{ member.img | relative_url }}" alt="member thumbnail">
        {% endif %}
        <div class="card-body">
          <h3 class="card-title">{{ member.title }}</h3>
          <p class="card-text">{{ member.description }}</p>
          <div class="row ml-1 mr-1 p-0">
            {% if member.email %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.email }}" target="_blank"><i class="far fa-envelope gh-icon"></i></a>
            </div>
            {% endif %}
            {% if member.github %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
            </div>
            {% endif %}
            {% if member.linkedin %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.linkedin }}" target="_blank"><i class="fab fa-linkedin gh-icon"></i></a>
            </div>
            {% endif %}
            {% if member.scholar %}
            <div class="icon" data-toggle="tooltip" title="Code Repository">
              <a href="{{ member.scholar }}" target="_blank"><i class="fas fa-graduation-cap gh-icon"></i></a>
            </div>
            {% endif %}
          </div>
        </div>
      </div>
    {% if member.personal_url %}
    </a>
    {% endif %}
  </div>
  {% endif %}
{% endfor %}

</div>
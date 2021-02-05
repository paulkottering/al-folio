---
layout: page
title: research + coursework
permalink: /projects/
description: Some projects I have completed. Coursework completed listed below.
nav: true
---

<div class="projects grid">

  {% assign sorted_projects = site.projects | sort: "importance" %}
  {% for project in sorted_projects %}
  <div class="grid-item">
    {% if project.redirect %}
    <a href="{{ project.redirect }}" target="_blank">
    {% else %}
    <a href="{{ project.url | relative_url }}">
    {% endif %}
      <div class="card hoverable">
        {% if project.img %}
        <img src="{{ project.img | relative_url }}" alt="project thumbnail">
        {% endif %}
        <div class="card-body">
          <h2 class="card-title text-lowercase">{{ project.title }}</h2>
          <p class="card-text">{{ project.description }}</p>
          <div class="row ml-1 mr-1 p-0">
            {% if project.github %}
            <div class="github-icon">
              <div class="icon" data-toggle="tooltip" title="Code Repository">
                <a href="{{ project.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
              </div>
              {% if project.github_stars %}
              <span class="stars" data-toggle="tooltip" title="GitHub Stars">
                <i class="fas fa-star"></i>
                <span id="{{ project.github_stars }}-stars"></span>
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
<br/>

Here is some selected advanced undergraduate coursework which I have completed:

| UC Berkeley Course Number      | Subject | Class Description     |
| :---        |    :----:   |          ---: |
221	|Math|	Advanced Matrix Computations
185|	Math|	Introduction to Complex Analysis
170|	Math	|Mathematical Methods for Optimization
128A |	Math|	Numerical Analysis
113	|Math|	Introduction to Abstract Analysis
110	|Math|	Linear Algebra
104|	Math|	Introduction to Analysis
C191 |	Physics	|Quantum Information Science and Technology
188	|Physics	|     Bayesian Data Analysis and Machine Learning for Physical Sciences
151	|Physics	|Elective Physics: Quantum Field Theory
137B	|Physics|	Quantum Mechanics Part B
137A |	Physics	|Quantum Mechanics Part A
105	|Physics	|Analytic Mechanics

---
layout: archive
title: "Curriculum vitae"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
  - /curriculumvitae
---

{% include base_path %}

## Education

* B.Eng. in Engineering Physics, McMaster University, Hamilton, ON, Canada, 2007
* Ph.D in Engineering Physics, McMaster University, Hamilton, ON, Canada, 2014
  * Thesis : An Investigation into the Role of Energy and Symmetry at Epitaxial Interfaces
  * Advisor: Dr. John S. Preston

## Publications

  <ul>{% for post in site.publications %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
## Talks

  <ul>{% for post in site.talks %}
    {% include archive-single-talk-cv.html %}
  {% endfor %}</ul>
  
## Teaching

  <ul>{% for post in site.teaching %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
## Service and leadership

* Software Carpentry, Online, 2012&ndash;Present
  * Maintainer and Developer, shell-novice lessons
  * Certified Instructor

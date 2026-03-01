---
layout: default
permalink: /blog/
title: miscellaneous things
nav: true
nav_order: 1
pagination:
  enabled: true
  collection: posts
  permalink: /page/:num/
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="post">

{% assign blog_name_size = site.blog_name | size %}
{% assign blog_description_size = site.blog_description | size %}

{% if blog_name_size > 0 or blog_description_size > 0 %}
  <div class="header-bar blog-page-header">
    <h1>{{ site.blog_name }}</h1>
    <h2>{{ site.blog_description }}</h2>
  </div>
{% endif %}

{% if page.pagination.enabled %}
  {% assign postlist = paginator.posts %}
{% else %}
  {% assign postlist = site.posts %}
{% endif %}

<div class="container featured-posts">
  <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3">
    {% for post in postlist %}
      {% if post.external_source == blank %}
        {% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
      {% else %}
        {% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
      {% endif %}
      {% assign year = post.date | date: "%Y" %}
      <div class="col mb-4">
        {% if post.redirect == blank %}
          <a href="{{ post.url | relative_url }}">
        {% elsif post.redirect contains '://' %}
          <a href="{{ post.redirect }}" target="_blank">
        {% else %}
          <a href="{{ post.redirect | relative_url }}">
        {% endif %}
          <div class="card hoverable">
            <div class="card-body">
              {% if post.featured %}
                <div class="float-right"><i class="fa-solid fa-thumbtack fa-xs"></i></div>
              {% endif %}
              <h3 class="card-title text-lowercase">{{ post.title }}</h3>
              <p class="card-text">{{ post.description }}</p>
              <p class="post-meta">
                {{ read_time }} min read &nbsp;&middot;&nbsp;
                {{ post.date | date: '%B %d, %Y' }}
                &nbsp;&middot;&nbsp;
                <a href="{{ year | prepend: '/blog/' | prepend: site.baseurl}}"><i class="fa-solid fa-calendar fa-sm"></i> {{ year }}</a>
                {% if post.external_source %}&nbsp;&middot;&nbsp; {{ post.external_source }}{% endif %}
              </p>
            </div>
            {% if post.thumbnail %}
              <img class="card-img-bottom" src="{{ post.thumbnail | relative_url }}" style="object-fit: cover; height: 140px" alt="">
            {% endif %}
          </div>
        </a>
      </div>
    {% endfor %}
  </div>
</div>

{% if page.pagination.enabled %}
  {% include pagination.liquid %}
{% endif %}

</div>

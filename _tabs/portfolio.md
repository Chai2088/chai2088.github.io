---
layout: page
icon: fas fa-code
order: 1
permalink: /portfolio/
---
During my four years in college I have worked in several projects, including personal and school projects. Here I have posted some of coolest projects I have worked on.

<div class="card-list">
{% assign sorted_posts = site.articles | sort: "order" %}
  {% for post in sorted_posts %}
    <div class="card post-preview mb-4 bg-transparent position-relative border-light">
      <!-- Clickable overlay -->
      <a href="{{ post.url | relative_url }}" class="stretched-link z-1"></a>
      
      <!-- Light image overlay -->
      {% if post.image %}
      <div class="card-img-top bg-white bg-opacity-10" 
           style="height: 180px; 
                  background: linear-gradient(rgba(255,255,255,0.3), rgba(255,255,255,0.2)), 
                             url('{{ post.image | relative_url }}') center/cover;">
      </div>
      {% endif %}
      
      <div class="card-body position-relative">
        <div class="position-relative z-2">
          <!-- Light blue title -->
          <h2 class="card-title h4" style="color: #6c8dfa !important;">{{ post.title }}</h2>
          <!-- Light gray text -->
          <div class="card-text post-content" style="color: #a0aec0 !important;">
            {{ post.excerpt | truncate: 120 | strip_html }}
          </div>
        </div>
      </div>
      
      <!-- Light categories -->
      {% if post.categories %}
      <div class="card-footer bg-transparent border-top-0 pt-0 position-relative">
        <div class="position-relative z-2">
          <div class="post-meta small" style="color: #8e9aaf !important;">
            {{ post.categories | join: ", " }}
          </div>
        </div>
      </div>
      {% endif %}
    </div>
  {% endfor %}
</div>


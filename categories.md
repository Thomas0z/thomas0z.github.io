---
layout: page
title: Categories
permalink: /categories/
---

<div class="categories-page">
  <div class="category-list">
    {% for category in site.categories %}
      <div class="category-group">
        <div class="category-header">
          <h2 id="{{ category[0] }}">{{ category[0] | capitalize }}</h2>
          <span class="post-count">{{ category[1].size }} posts</span>
        </div>
        
        <ul class="post-list">
          {% for post in category[1] %}
            <li>
              <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
              <a href="{{ post.url | relative_url }}" class="post-link">{{ post.title }}</a>
            </li>
          {% endfor %}
        </ul>
      </div>
    {% endfor %}
  </div>
</div>

<style>
.categories-page {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}


.category-group {
  margin-bottom: 40px;
}

.category-header {
  border-bottom: 2px solid #eee;
  margin-bottom: 15px;
  padding-bottom: 5px;
  display: flex;
  align-items: baseline;
}

.category-header h2 {
  margin: 0;
  color: #333;
  font-size: 1.8em;
}

.post-count {
  margin-left: 15px;
  color: #666;
  font-size: 0.9em;
}

.post-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.post-list li {
  margin: 12px 0;
  padding: 8px;
  transition: background-color 0.2s;
}

.post-list li:hover {
  background-color: #f8f9fa;
  border-radius: 4px;
}

.post-date {
  color: #666;
  font-size: 0.9em;
  margin-right: 15px;
}

.post-link {
  color: #2c3e50;
  text-decoration: none;
}

.post-link:hover {
  color: #1abc9c;
}

@media (max-width: 600px) {
  .category-header {
    flex-direction: column;
  }
  
  .post-count {
    margin-left: 0;
    margin-top: 5px;
  }
  
  .post-date {
    display: block;
    margin-bottom: 5px;
  }
}
</style>
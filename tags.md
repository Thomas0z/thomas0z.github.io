---
layout: page
title: Tags
permalink: /tags/
---
<div class="tags-page">
  <div class="tag-cloud">
    {% assign max_count = 1 %}
    {% for tag in site.tags %}
      {% if tag[1].size > max_count %}
        {% assign max_count = tag[1].size %}
      {% endif %}
    {% endfor %}
    
    {% for tag in site.tags %}
      {% assign size_factor = tag[1].size | times: 100 | divided_by: max_count %}
      {% assign color_index = tag[1].size | modulo: 5 %}
      {% assign tag_word = tag | first %}
      <a href="#{{ tag_word | slugify }}" 
         class="tag-link color-{{ color_index }}" 
         data-count="{{ tag[1].size }}"
         style="font-size: calc(0.9em + {{ size_factor }}%/80);">
        {{ tag_word }}
        <span class="tag-count">{{ tag[1].size }}</span>
      </a>
    {% endfor %}
  </div>

  <div class="tag-list">
    {% for tag in site.tags %}
      {% assign tag_word = tag | first %}
      <div class="tag-group" id="{{ tag_word | slugify }}">
        <div class="tag-header">
          <h2>{{ tag_word }}</h2>
          <span class="post-count">{{ tag[1].size }} posts</span>
        </div>
        
        <ul class="post-list">
          {% assign tagged_posts = site.tags[tag_word] | sort: 'date' | reverse %}
          {% for post in tagged_posts %}
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
.tags-page {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.tag-cloud {
  margin-bottom: 40px;
  padding: 30px;
  background: #f8f9fa;
  border-radius: 8px;
  text-align: center;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
}

.tag-link {
  display: inline-flex;
  align-items: center;
  padding: 8px 16px;
  border-radius: 20px;
  text-decoration: none;
  transition: all 0.3s ease;
  font-weight: 500;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.tag-link.color-0 {
  background: #E3F2FD;
  color: #1565C0;
}

.tag-link.color-1 {
  background: #E8F5E9;
  color: #2E7D32;
}

.tag-link.color-2 {
  background: #FFF3E0;
  color: #E65100;
}

.tag-link.color-3 {
  background: #F3E5F5;
  color: #6A1B9A;
}

.tag-link.color-4 {
  background: #E0F7FA;
  color: #00838F;
}

.tag-link:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.tag-link.color-0:hover { background: #BBDEFB; }
.tag-link.color-1:hover { background: #C8E6C9; }
.tag-link.color-2:hover { background: #FFE0B2; }
.tag-link.color-3:hover { background: #E1BEE7; }
.tag-link.color-4:hover { background: #B2EBF2; }

.tag-count {
  font-size: 0.75em;
  margin-left: 8px;
  padding: 2px 6px;
  border-radius: 10px;
  background: rgba(255,255,255,0.5);
}

.tag-group {
  margin-bottom: 40px;
  scroll-margin-top: 20px;  /* 添加滚动边距 */
}

.tag-header {
  border-bottom: 2px solid #eee;
  margin-bottom: 15px;
  padding-bottom: 5px;
  display: flex;
  align-items: baseline;
}

.tag-header h2 {
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
  .tag-cloud {
    padding: 15px;
    gap: 8px;
  }

  .tag-link {
    padding: 6px 12px;
    font-size: 0.85em !important;
  }

  .tag-header {
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
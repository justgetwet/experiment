---
layout: page
title: "Latest"
permalink: latest.html
---

{% for post in site.posts %}
  <aside>
    <div class="article-head">
      <div>{% include icons/calendar.svg %}</div>
      <h5>{{ post.date | date: "%d %b %Y"}}</h5>
      <div>{% include icons/tag.svg %}</div>
      <h5>{{ post.tags | join: " "}}</h5>
    </div>
    <h3>
      <div class="post-title">
        <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
      </div>
    </h3>
    <div class="excerpt">
      {{ post.excerpt }}
    </div>
    <hr />
  </aside>  
{% endfor %}

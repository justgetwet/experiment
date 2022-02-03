---
layout: page
---

<div class="posts_head">
  {% include icons/home.svg %}
  <h3>Latest Posts</h3>
</div>
{% for post in site.posts %}
  <aside>
    <section>
      <div class="article-head">
        <div>{% include icons/calendar.svg %}</div>
        <h4>{{ post.date | date: "%d %B %Y"}}</h4>
        <div>{% include icons/tag.svg %}</div>
        <h4>{{ post.tags | join: " "}}</h4>
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
    </section>
  </aside>
{% endfor %}

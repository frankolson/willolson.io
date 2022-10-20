---
layout: default
---

{% assign grouped_posts = site.posts | group_by_exp: "item", "item.date | date: '%B %Y'" %}

# All articles

I write about web development, cryptography, and other topics I find interesting.

{% for month_group in grouped_posts %}
  <h3>{{ month_group.name }}</h3>

  <ul>
    {% for post in month_group.items %}
      <li>
        <a href="{{ post.url | prepend: site.baseurl }}">
          {{ post.title }}
        </a>
      </li>
    {% endfor %}
  </ul>
{% endfor %}
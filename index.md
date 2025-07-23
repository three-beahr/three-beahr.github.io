---
layout: default
title: Homepage
---

# Welcome to our club, welcome to our club, (welcome Squidward! welcome Squidward! welcome Squidward!...)

I'm learning Haskell! While I do love my current job, it has become apparent to me that applying my math & programming skills to the world of trading would be a real dream for me. These blog posts will serve as a little journal of my time learning about Haskell and the projects I create. 

---

## 📚 Recent Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <small>({{ post.date | date: "%B %d, %Y" }})</small>
    </li>
  {% endfor %}
</ul>
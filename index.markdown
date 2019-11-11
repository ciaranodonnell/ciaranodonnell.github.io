---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

<div>
  {% for post in site.posts %}
    
      <a href="{{ post.url }}"><h1>{{ post.title }}</h1></a>
    <p>  {{ post.excerpt }}
    </p>
  {% endfor %}
  <div>

---
layout: blog/default
permalink: /blog/
---

<section class="posts">
<ul>
{% for post in site.posts %}
<li><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%Y-%m-%d" }}</time></li><li class="tags">
{% for tag in post.tags %}
<a href="/tag/{{tag}}/">{{tag}}</a> 
{% endfor %}
</li>
{% endfor %}
</ul>
</section>


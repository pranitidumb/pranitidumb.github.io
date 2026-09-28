---
layout: home
title: Blog
permalink: /blog/
---
<div class="blog-intro">
  <p>For a long time, writing was straightforward: you thought, and you wrote. You read it several years later and either hated your writing with a burning passion or felt deeply in awe of the sheer magnitude of profound thought that the proclivities of your mind were capable of producing. This has been a consistent part of my lived experience.</p>
  <p>Writing is a dying skill, both at large and for me. This blog is an attempt to preserve it, so that, ten years from now, I can look back and either say, “My writing is not meant for human eyes; I should disinfect my eyes ASAP,” or, “Who is Dostoevsky, and why does he have absolutely nothing on me?”</p>
</div>

{% assign welcome_post = site.posts | last %}
{% if welcome_post %}
<div class="links-row blog-cta">
  <a class="link-pill navy" href="{{ welcome_post.url | relative_url }}">Welcome to my blog</a>
</div>
{% endif %}

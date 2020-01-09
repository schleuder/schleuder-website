---
layout: news
title:  "News"
---
{% for post in site.posts %}

## {{ post.title }}

### {{ post.date | date_to_string: "ordinal", "US" }}
  
{{ post.excerpt }}

[More]({{ post.url }})

{% endfor %}

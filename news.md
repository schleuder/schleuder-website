---
layout: news
title:  "News"
---
{% for post in site.posts %}

## [{{ post.title }}]({{ post.url }})

### {{ post.date | date_to_string: "ordinal", "US" }}
  
{{ post.excerpt }}

{% endfor %}

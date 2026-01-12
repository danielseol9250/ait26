---
layout: home
---

# IT 블로그에 오신 것을 환영합니다!

이 블로그는 IT 기술, 개발, 프로그래밍에 관한 글을 공유하는 공간입니다.

## 최근 포스트

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%Y년 %m월 %d일" }}
{% endfor %}


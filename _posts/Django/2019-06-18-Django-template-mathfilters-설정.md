---
layout: post
title: django template 연산 안될 때 - mathfilters 설정
category: Django
tags: [django, python, python-lib]
comments: true
---

# [django] django template 연산 안될 때 - mathfilters 설정

## [1] 라이브러리 받기

`pip install django-mathfilters` 설치 



## [2] settings.py 추가

mathfilters를 apps에 추가해줘야한다

```python
INSTALLED_APPS = [
    ...
    'mathfilters',
    ...
]
```



## [3] html파일에 load해주기

math 연산을 사용할 html 파일에 load를 해줘야 한다.

<center>
<figure>
<img src="/assets/post-img/django/1561119420114.png" alt="views">
<figcaption>브라우저에서 확인하기</figcaption>
</figure>
</center>

## [4] 간단한 연산 코드

```html
<!-- 더하기 -->
{{ 10 | add:1 }}
<!-- 빼기 -->
{{ 10 | sub:1 }}
<!-- 곱하기 -->
{{ 10 | mul:2 }}
<!-- 나누기 -->
{{ 10 | div:2 }}
```






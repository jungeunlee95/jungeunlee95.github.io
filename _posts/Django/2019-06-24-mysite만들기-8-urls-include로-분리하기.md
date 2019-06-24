---
layout: post
title: Django-mysite만들기8 URL 간결화 - namespace(app_name주기)
category: Django
tags: [django, python, django-orm]
comments: true
---

---

[전체 코드 보기](https://github.com/jungeunlee95/python-mysite)

---

[지난포스팅]([https://jungeunlee95.github.io/django/2019/06/23/mysite%EB%A7%8C%EB%93%A4%EA%B8%B0-7-%EA%B3%84%EC%B8%B5%ED%98%95-%EB%8B%B5%EA%B8%80-%EA%B2%8C%EC%8B%9C%ED%8C%90-%EA%B8%B0%EB%8A%A5-%EC%B6%94%EA%B0%80/](https://jungeunlee95.github.io/django/2019/06/23/mysite만들기-7-계층형-답글-게시판-기능-추가/))

---

Django mysite를 만들면서, application이 늘어나면서

project의 urls.py가 복잡해졌다.

```python
urlpatterns = [
    path('', main_views.index),

    path('user/joinform', user_views.joinform),
    path('user/joinsuccess', user_views.joinsuccess),
    path('user/join', user_views.join),

    path('user/api/checkemail', user_views.checkemail),

    path('user/loginform', user_views.loginform),
    path('user/login', user_views.login),
    path('user/logout', user_views.logout),

    path('user/updateform', user_views.updateform),
    path('user/update', user_views.update),

    path('guestbook/list', guestbook_views.list),
    path('guestbook/write', guestbook_views.write),
    path('guestbook/delete/<int:no>', guestbook_views.delete),
    path('guestbook/delete', guestbook_views.delete),

    path('board/list/<int:page>', board_views.list),
    path('board/list/', board_views.list),
    path('board/writeform', board_views.writeform),
    path('board/writeform/<int:no>', board_views.writeform),
    path('board/write', board_views.write),
    path('board/<int:no>', board_views.view),
    path('board/modify/<int:no>', board_views.modifyform),
    path('board/modify', board_views.modify),
    path('board/delete/<int:no>', board_views.delete),


    path('admin/', admin.site.urls),
]
```

너무 많은 url 매핑들...! 

> 복잡해진 코드를 해결하기 위해서 각각의 Django App 안에 urls.py 파일을 만들고, 
>
> 메인 urls.py 파일에서 각 Django App의 urls.py 파일로 URL 매핑을 위탁하게 코드를 수정하자!

---

## [1] 각각의 App안에 urls.py 만들기

<center>
<figure>
<img src="/assets/post-img/django/1561357177358.png" alt="views">
<figcaption></figcaption>
</figure>
</center>
---



## [2] 각각 App에서 urls.py 매핑해주기

맨 위의 아주 긴~ urls.py를 각각의 app매핑에 따라서 urls.py로 나눠 배치해준다.

**ex) board기능**

**board/urls.py**

```python
from django.urls import path
from . import views # 해당 app의 views import!

urlpatterns = [
    # path는 main에서 `board/` url을 위탁시키기에 그 뒤의 주소부터 넣어준다.
    path('list/<int:page>', views.list),
    path('list/', views.list),
    path('writeform', views.writeform),
    path('writeform/<int:no>', views.writeform),
    path('write', views.write),
    path('<int:no>', views.view),
    path('modify/<int:no>', views.modifyform),
    path('modify', views.modify),
    path('delete/<int:no>', views.delete),
]
```

이렇게 모든 app의 각각의 urls.py를 만들어 코드를 나눠주면 된다.

---



## [3] 메인 urls.py에서 include 해주기

include모듈을 import해준다.

> `from django.urls import include` 

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('main.urls')),
    path('user/', include('user.urls')),
    path('guestbook/', include('guestbook.urls')),
    path('board/', include('board.urls')),
    path('admin/', admin.site.urls),
]
```

main urls.py의 코드가 아주 간결해졌다!

---



## [4] URL 간결화시키기 (app_name 주기)

각각의 app의 urls.py를 분리시킨 뒤, 

templates html에서 사용하는 url를 간결화를 위해서

`app_name`과 각각 url path에 name을 줄 수 있다.

ex) **board/urls.py**

```python
from django.urls import path
from . import views

app_name = 'board'

urlpatterns = [
    path('list/<int:page>', views.list, name='list_page'),
    path('list/', views.list, name='list'),
    path('writeform', views.writeform, name='writeform'),
    path('writeform/<int:no>', views.writeform, name='reply_writeform'),
    path('write', views.write, name='write'),
    path('<int:no>', views.view, name='view'),
    path('modify/<int:no>', views.modifyform, name='modifyform'),
    path('modify', views.modify, name='modify'),
    path('delete/<int:no>', views.delete, name='delete'),
]
```

> 언뜻보면, urls.py 코드가 길어져 더 복잡해 보일 수 있지만,
>
> 이렇게 각각의 name을 주고 나면, url 매핑을 할때 하드코딩을 피할 수 있게 된다. 



**ex) board/write.html**

<center>
<figure>
<img src="/assets/post-img/django/1561358534474.png" alt="views">
<figcaption></figcaption>
</figure>
</center>



### 파라미터 보내기

위의 **urls.py**에서, 

`path('writeform/<int:no>', views.writeform, name='reply_writeform'),` 

위와 같이 파라미터를 보내는 경우 어떻게 처리해야할까?

<center>
<figure>
<img src="/assets/post-img/django/1561358850749.png" alt="views">
<figcaption></figcaption>
</figure>
</center>
---



## [5] views에서도 간결한 처리 가능

redirect로 처리할 경우

`return redirect('board:view', article_id)`

이런식으로 처리할 수 있다.












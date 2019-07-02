---
layout: post
title: 9 리눅스에서 mod_wsgi로 apache와 django 연동(PYTHONPATH 설정)
category: Linux
tags: [linux, VirtualBox, centos, apache, Python]
comments: true
---

---

## [1] mod_wsgi 설치하기

[다운로드 링크](https://github.com/GrahamDumpleton/mod_wsgi/releases/tag/4.6.4)

<center>
<figure>
<img src="/assets/post-img/linux/1562058410092.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

다운 : `# wget https://github.com/GrahamDumpleton/mod_wsgi/archive/4.6.4.tar.gz`

이름변경 : `# mv 4.6.4.tar.gz mod_wsgi_4.6.4.tar.gz`

압축 해제: `# tar xvfz mod_wsgi_4.6.4.tar.gz`

dir 들어가기 : `# cd mod_wsgi-4.6.4/`

빌드환경 설정 : `# ./configure --with-apxs=/usr/local/cafe24/apache/bin/apxs --with-python=/usr/local/cafe24/python3.7/bin/python3`

```
--with-apxs : 모듈 만드는 아파치 도구
--with-python : 파이썬 라이브러리 위치 알려주기
```

`# make`

`# make install`

<br>

## [2] 환경 설정

### 1) httpd.conf 설정

모듈 로딩 위치 : apache설치 경로의 `conf/`

`[root@localhost apache]# vi conf/httpd.conf`

 /LoadMod 검색해서 LoadModule맨 아래에 `LoadModule wsgi_module modules/mod_wsgi.so` 추가

<center>
<figure>
<img src="/assets/post-img/linux/1562062959169.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

### 2) extra/httpd-vhosts.conf

8888 포트의 VirtualHost 설정하기

`[root@localhost apache]# vi conf/extra/httpd-vhosts.conf`

<center>
<figure>
<img src="/assets/post-img/linux/1562063028630.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

### 3) git에서 배포할 project clone해오기

apache에 배포할 프로젝트를 git에서 clone해 올 것이다.

먼저 해당 프로젝트에서  `pip3 freeze > requirements.txt` 만들어서 git에 올리기

<br>

프로젝트 다운 받을 경로 : `/home/django`

프로젝트 끌어오기 : `# git clone https://github.com/jungeunlee95/django-basic.git`

설정 이름으로 변경: `# mv django-basic/ python_ch3`

`# cd python_ch3`

가상환경 만들기 : `# virtualenv venv`

가상환경 실행 : `# source venv/bin/activate`  

라이브러리 설치 : `(venv) [root@localhost python_ch3]# pip3 install -r requirements.txt `

<br>

**port 9999 열기**

`# vi /etc/sysconfig/iptables`

​    `-A INPUT -m state --state NEW -m tcp -p tcp --dport 9999  -j ACCEPT` 추가

`# /etc/init.d/iptables restart` : 재시작

> 해당프로젝트의 ip허용
>
> settings.py의 `ALLOWED_HOSTS = ['*']` 설정해주기
>
> ==> 로컬에서 수정하였으면, git에 commit 후 다시 리눅스에서 pull해오기

<br>

### 4) 서버 실행하기

`# python manage.py runserver 0.0.0.0:9999`

하지만 여기서 <b style="color:blue">오류</b>가 발생했다.

> <center>
> <figure>
> <img src="/assets/post-img/linux/1562063338005.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>
>
> 바로 `undefined symbol : PQescapeIndentifier` -> psycopg 모듈을 찾을 수 없다는 오류.
>
> 하지만 해당 프로젝트의 가상환경 안에는 psycopg모듈이 잘 들어가있음을 확인했다.
>
> [여기서 오류 이유](http://www.leeladharan.com/importerror-psycopg-so:-undefined-symbol:-lo-truncate64)를 발견하고 해결할 수 있었다. (libpq 링크가 잘못 걸려있었다.)
>
> <b style="color:red">해결방법</b>
>
> ```
> # cd /usr/lib64
> # rm libpq.so.5  											# 링크삭제
> # ln -s /usr/local/cafe24/pgsql/lib/libpq.so.5 libpq.so.5   # 링크다시걸기
> # /etc/init.d/postgres stop								 	# 서버 재시작
> # /etc/init.d/postgres start
> ```

<center>
<figure>
<img src="/assets/post-img/linux/1562063533179.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

접속 성공!!!!!

<br>

---

## 가상환경 영향 받지 않고 runserver 시키기

해당 프로젝트의 가상환경에서 나오고 프로젝트 상위폴더에서

`python3 python_ch3/manage.py runserver 0.0.0.0:9999` 를 입력하면 서버가 실행되지 않는다.

<center>
<figure>
<img src="/assets/post-img/linux/1562063635430.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

만약 가상환경을 실행하지 않고 django를 실행하고 싶다면,

venv안에 있는 lib를 프로젝트 바로 밑에 install해주면 된다.

`# pip3 install -r python_ch3/requirements.txt --target=python_ch3`

<center>
<figure>
<img src="/assets/post-img/linux/1562063717919.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

> 서버 실행 성공!!!

<br>

## apache-django 연동 연습 (PYTHONPATH 설정 ⭕)

프로젝트 당겨오기 : `# git clone https://github.com/jungeunlee95/django-basic.git`

설정 이름으로 변경 : `# mv django-basic/ python_ch3`

프로젝트 바로 밑에 lib 다운: `# pip3 install -r python_ch3/requirements.txt --target=/home/django/python_ch3`

PYTHONPATH 지정 : `# export PYTHONPATH='/home/django/python_ch3'`

서버 재시작 : `# /etc/init.d/httpd restart`

<center>
<figure>
<img src="/assets/post-img/linux/1562063756549.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

## apache-django 연동 연습 (PYTHONPATH 설정 ❌)

프로젝트 당겨오기 :`# git clone https://github.com/jungeunlee95/django-basic.git`

설정 이름으로 변경 :`# mv django-basic/ python_ch3`

프로젝트 바로 밑에 lib 다운: `# pip3 install -r /home/django/python_ch3/requirements.txt --target=/home/django/python_ch3`

PYTHONPATH 지정 해제 :`# export PYTHONPATH=''`

서버 재시작 : `# /etc/init.d/httpd restart`

<br>

이렇게 PYTHONPATH를 없애면 오류가 발생한다.

<b style="color:blue">오류 확인해보기</b>

로그 확인하기 : 해당 apache 설치 경로의 `/logs/`에서 확인할 수 있다.

<center>
<figure>
<img src="/assets/post-img/linux/1562064018040.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

> ModuleNotFoundError : No module named 'django'
>
> 장고 모듈을 확인할 수 없다.

이 오류는 장고 프로젝트의 `wsgi.py`에서 환경변수 설정 코드를 추가해야 해결할 수 있다.

```python
import sys
# 프로세스 내에서만 사용하는 환경변수(프로세스가 내려가면 사라지는 path)
sys.path.append(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```

위의 코드를 넣어주고 서버를 재시작하면

<center>
<figure>
<img src="/assets/post-img/linux/1562064188385.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

접근 성공!!!!



<br>

<br>

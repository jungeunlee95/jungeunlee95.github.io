---
layout: post
title: 5 linux에 파이썬 가상 개발 환경 구성하기(Virtual Environments)
category: Linux
tags: [linux, VirtualBox, centos, Xshell, Python]
comments: true
---

---

### ✔  2.4 리눅스에서 python 프로젝트 git clone 해오기 

#### 1, pycharm에서 upgsql_crud 프로젝트 생성

굳이 파이참이 아니여도 상관 없다. ( 파이참이 아니라면 가상환경을 직접 만들어 주어야 한다.)

(이미 만든 프로젝트가 있다면 pass)

<br>

#### 2, 패키지 의존성 파일 생성

> `pip freeze > requirements.txt`
>
> 해당 프로젝트의 모듈 정보를 txt파일로 만들어준다.
>
> ```txt
> psycopg2==2.8.3
> ```

<br>

#### 3, github commit (venv 및 프로젝트 설정 파일 ignore)

나는 pycharm프로젝트를 사용하는데, 여기서 `.idea`와 `venv`를 체크 해제 하거나 Ignore을 선택해 주면 된다.

pycharm을 사용하지 않더라도 git add를 할때 저 둘을 제외하고 add해주면 된다.

> ignore all files under 선택

> <center>
> <figure>
> <img src="/assets/post-img/linux/1561955064828.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>
>
> git push를 해준다.

<br>

#### 4, 리눅스에서 git clone 받기

해당 git 주소를 이제 `python-projects` 밑에 clone 해 올 것이다.

`git clone https://github.com/jungeunlee95/upgsql_crud.git`

<center>
<figure>
<img src="/assets/post-img/linux/1561954594827.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

#### 5, 가상환경 생성(isolationize)

`[root@localhost python-projects]# cd upgsql_crud`

`[root@localhost upgsql_crud]# virtualenv venv`   : 가상환경 만들기

`# source venv/bin/activate`										   : 가상환경 실행

<br>

#### 6, 의존성 파일 설치

기존 python프로젝트에서 만들어준 의존성 파일을

linux 가상환경에 설치해줘야한다. (공유)

`(env) [root@localhost upgsql_crud]# pip install -r requirement.txt`

> <center>
> <figure>
> <img src="/assets/post-img/linux/1561955699216.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>
>
> **설치를 한 뒤 해당 프로젝트 가상환경의 설치 경로에 가면 설치가 된 것을 확인 할 수 있다.**

<br>

#### 7, 실행

> <center>
> <figure>
> <img src="/assets/post-img/linux/1561956082215.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>
>
> 성공!

---



#### local에서 리눅스 python프로젝트를 pycharm에 git clone 해온다면!?

로컬에서 git clone을 해온 뒤 pycharm의 project interpreter에서 venv를 설정해주면 된다.

**settings -> project interpretor**

<center>
<figure>
<img src="/assets/post-img/linux/1561956440240.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

terminal에서 가상환경을 실행한 뒤 의존성 파일을 실행하면 된다.

`call venv/scripts/activate`

 `pip install -r requirement.txt`

`pip list`     : 리스트 확인하기

---



### ✔  2.5 upgsql_crud프로젝트의 test_crud.py를 고립시키지 않고(venv 사용 X) 실행하기

해당 프로젝트의 가상환경 아래 루트가 sys path에 걸려있으면 된다.

**가상환경 아래의 루트 확인하기**
> 가상환경의 python3를 실행한뒤 sys.path를 찍어본다.
>
> <center>
> <figure>
> <img src="/assets/post-img/linux/1561958130896.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>



`# cd /root/dowork/python-projects`

`# export PYTHONPATH='/root/dowork/python-projects/upgsql_crud/venv/lib/python3.7/site-packages'`  : path 등록

<br>

위처럼 설정을 해준다면, 이제 가상환경을 실행하지 않아도 실행할 수 있게 된다.

실행 : `[root@localhost python-projects]# python3 upgsql_crud/test_crud.py`

---

<br>

<br>

<br>

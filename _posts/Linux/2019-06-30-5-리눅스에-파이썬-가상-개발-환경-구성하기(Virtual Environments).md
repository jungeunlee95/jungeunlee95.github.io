---
layout: post
title: 5 linux에 파이썬 가상 개발 환경 구성하기(Virtual Environments)
category: Linux
tags: [linux, VirtualBox, centos, Xshell, Python]
comments: true
---

---

<center>
<figure>
<img src="/assets/post-img/linux/1561970278198.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

---

## Python Isolation Tools (Virtual Environments)

python의 가상환경 툴은 여러가지가 있다.

<b style="color:blue">파이썬에서 가상환경을 쓰는 이유?</b>

> 파이썬은 파이썬 버전 별로 다수의 서브 버전이 존재하며, 엄청난 수의 패키지를 만들고 공유하고있다. 또 이런 패키지들은 개별적인 버전들을 갖고 있다.
>
> 만약 컴퓨터 한대에서 여러개의 프로젝트를 만들어서 프로그램을 돌릴경우, 파이썬 애플리케이션의 런타임 버전과 라이브러리 충돌이 일어나면 문제가 발생할 것이다.
>
> 이 문제가 발생하는 이유는 파이썬 버전과 라이브러리 패키지 등이 전역적으로 설치되어서 사용되기 때문이다.
>
> 이를 해결하기 위하여 파이썬은 각각 애플리케이션 별로 독립적인 환경을 만들어줘야한다.

파이썬 가상 환경을 구성하기 위해서는 여러가지 툴이 필요하다.

```
1. virtualenv : python2 부터 사용하던 가상환경 라이브러리
2. venv 	  : python 3.3 이후부터 기본 모듈
3. pyenv	  : Python Interpretor Version Manager
4. conda	  : Anaconda Python 설치했을 때 사용할 수 있다.
5. etc		  : virtualenvwrapper, buildout ...
```

먼저 [1.virtualenv]와 [2.venv]로 가상환경을 구성하는 법에 대해서 알아보았다.

<br>

### ✔ 1. virtualenv로 가상환경 구성하기

#### 1, 설치

원래 python의 모듈 실행은 `python3 -m pip` 이렇게 해야하는데, 스크립트 제공해줘서 `pip3` or `pip`로 사용할 수 있다.

상위로 위치를 옮기기 :  `# cd`

virtualenv 설치하기    : `# pip3 install virtualenv`

<br>

#### 2, 프로젝트 생성

`# cd dowork/`   	                 - 나의 project들을 관리 디렉토리

`# mkdir python-projects` - python project를 관리하는 디렉토리 생성하기

`# cd python-projects`        - 디렉토리로 들어가기

`# mkdir loganalysis`          - loganalysis라는 이름의 python project 생성하기

<br>

#### 3, 가상환경 생성

`# cd loganalysis`				- 프로젝트로 들어가기

`# virtualenv venv` 			- 가상환경 만들기

> : 전역에 있는 파이썬을(/cafe24/python3.7/) 우리 프로젝트에 venv라는 디렉토리 밑으로 복사한다.
>
> <center>
> <figure>
> <img src="/assets/post-img/linux/1561947266730.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

<br>

#### 4, 가상환경 구동

`# source venv/bin/activate`

<br>

#### 5, 가상환경 확인

`(venv) # python --version `

> 가상환경이 실행되면 제일 앞에 `(가상환경이름)` 이 붙게된다.

<br>

#### 6, 가상환경 나가기 : `# deactivate`

> <center>
> <figure>
> <img src="/assets/post-img/linux/1561947582631.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

<br>

### ✔ 2-2 venv로 가상환경 구성하기

`# deactivate`							  	: 이전의 loganalysis의 가상환경 나오기

`# cd ..`

`# mkdir loganalysis2` 			    : 새 프로젝트 만들기

`# cd loganalysis2/`

`# python3 -m venv venv`			 : 가상환경 만들기

`# source venv/bin/activate`	: 가상환경 실행

<br>

### ✔ 2.3 간단한 postgres 연결 모듈 테스트 해보기

`# cd loganalysis`						 	: 프로젝트 들어가기

`# source venv/bin/activate`		: 가상환경 실행

`# pip install psycopg2`				: psycopg2 모듈 설치

> 여기서 install한 psycopg2는 어디에 다운된걸까?
>
> `/root/dowork/python-projects/loganalysis/venv/lib/python3.7/site-packages`
>
> 해당 프로젝트 가상환경 밑에 다운이 되어있다.

<br>

**test를 위한 파이썬 파일 만들기**

> 해당 파일에는 미리 만들어 놓은 postgresql 연결 정보를 적어주었다.
>
> 각자 사용하는 db의 연결 정보를 넣어주면 된다.

`(venv) [root@localhost loganalysis]# vi test_connect.py`

```python
try:
    conn = psycopg2.connect(
        user='webdb',
        password='webdb',
        host='192.168.1.52',
        port='5432',
        database='webdb',
        )
    cursor = conn.cursor()
    cursor.execute('select version()')
    record = cursor.fetchone()
    print(f'connected to - {record}')

except Exception as e:
    print(f'error : {e}')

finally:
    'conn' in locals() \
    and conn \
    and conn.close()

    'cursor' in locals() \
    and cursor\
    and cursor.close()
```

<br>

💥 error가 발생했다.

<center>
<figure>
<img src="/assets/post-img/linux/1561948919230.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

> import 할 수 없다!?
>
> python path가 안잡혀서 에러가 나는 것이다.
>
> 이유 : pgsql 링크가 잘못 되어있었다.

> ⭐**postgres < 10.8인 경우 다음 사항 확인해야 할 것이다.**
>
> `# cd /usr/lib64`
>
> `# rm -f libpq.so.5` 				: 링크 삭제
>
> ` # ln -s /cafe24/pgsql/lib/libpq.so.5 libpq.so.5`  : 링크 걸기

<br>

에러 해결! :o:

```powershell
(venv) [root@localhost loganalysis]# python test_connect.py
connected to - ('PostgreSQL 10.2 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 4.4.7 20120313 (Red Hat 4.4.7-23), 64-bit',)
```

---



---



### ✔  2.4 리눅스에서 python 프로젝트 git clone 해오기 test

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

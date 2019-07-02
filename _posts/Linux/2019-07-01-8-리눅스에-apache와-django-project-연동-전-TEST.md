---
layout: post
title: 8 리눅스에 apache와 django project 연동 전 TEST
category: Linux
tags: [linux, VirtualBox, centos, apache, Python]
comments: true
---

---

## 시작하기 전,, 

#### [1] apache port 확인하는 방법

리눅스에 아파치가 다운되어있는 경로의 `conf/httpd.conf`에서 확인할 수 있다.

<center>
<figure>
<img src="/assets/post-img/linux/1562055020732.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

`/Listen` 으로 검색하기

 <center>
<figure>
<img src="/assets/post-img/linux/1562055041975.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

나는 80포트번호로 apache를 열어놓았다.

<br>

#### [2] apache와 tomcat AJP 연결 확인하는 방법

`[root@localhost apache]# vi conf/workers_jk.properties` 에서 확인할 수 있다.

<br>

#### [3]  apache와 tomcat 매핑된 url 확인하는 방법

`[root@localhost apache]# vi conf/uriworkermap.properties`

<br>

#### [4] tomcat 프로젝트 로그 확인하는 방법

tomcat에 연결된 프로젝트의 디렉토리의 `logs/`에서 확인할 수 있다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562055266062.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

이 중 보고싶은 로그 파일 보기,  -> catalina.out을 확인하면 된다.

`[root@localhost logs]# tail -4000f catalina.out`

<br>

#### [5] apache url 접근 확인하는 방법

apache 설치 경로의 logs를 확인하면 된다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562055375968.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

접근 url 보기 : `[root@localhost logs]# tail -3000f access_log `

error 로그 보기 : `[root@localhost logs]# tail -3000f error_log `

<br>

#### [6] tomcat과 연결된 프로젝트 port확인하는 방법

나는 이전에 tomcat-cafe24 폴더 밑에 여러가지 java 프로젝트를 넣어놨다.

프로젝트가 들어있는 tomcat의 `/conf/server.xml`을 살펴보면 확인할 수 있다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562055531524.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

vi로 들어간 뒤 `/Connector port`를 검색하면 찾을 수 있다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562055579565.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

> 현재 8080 port번호로 열려있음

<br>

#### [7] tomcat과 연결된 프로젝트 AJP port확인하는 방법

위와 똑같은 경로에서 `/AJP`를  검색하면 찾을 수 있다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562055623593.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

> 8009번으로 연결되어 있음

<br>

#### 내가 방화벽을 열고있는 port 번호 확인하는 방법

`vi /etc/sysconfig/iptables` 에서 확인 가능

 <center>
<figure>
<img src="/assets/post-img/linux/1562055721666.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

---

## ⭐ 리눅스에 아파치와 장고프로젝트 연동하기

apache-장고 에서는 장고 프로젝트의 `wsgi.py`파일로 실행된다.

리눅스 가상호스트를 이용해 다른 port를 사용해서 apache 멀티 포트 서비스를 사용했다.

(80 : java project, 8888: django project 사용)

### 아파치 Virtualhost 사용하기

#### 🌹 [1] httpd.conf 수정

아파치 설치 경로 밑의`conf/httpd.conf` 수정

 <center>
<figure>
<img src="/assets/post-img/linux/1562056109497.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

**1) post 8888 추가하기**

 <center>
<figure>
<img src="/assets/post-img/linux/1562056143781.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

**2) DocumentRoot 디렉토리 권한 설정 주석처리**

가상 호스트를 사용할 것이기 때문에 DocumentRoot는 주석을 해준다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562056156726.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

**3) Virtual Host 설정파일 주석 해제**

virtualhost 사용을 위해 httpd-vhosts.conf를 사용할 것이기에 주석을 해제한다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562056183195.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

#### 🌹 [2] httpd-vhosts.conf 설정

아파치 설치 경로 밑의`conf/extra/httpd-vhosts.conf` 설정

 <center>
<figure>
<img src="/assets/post-img/linux/1562056257953.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

**1) NameVirtualHost *:8888 추가**

 <center>
<figure>
<img src="/assets/post-img/linux/1562056321532.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

**2)  기존의 80 포트 vhosts경로 설정**

 <center>
<figure>
<img src="/assets/post-img/linux/1562056341522.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

**3) 8888 포트 vhosts경로 설정 - django project 설정**

 <center>
<figure>
<img src="/assets/post-img/linux/1562056399536.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

여기서 DocumentRoot에 설정해준 경로 `/home/django/`에 `python_ch3` 프로젝트를 만들어주자, 일단 git에서 장고 프로젝트를 가져오기전 test용으로 직접 디렉토리를 만들어서 테스트해보았다. ==> `index.html`만 만듦

 <center>
<figure>
<img src="/assets/post-img/linux/1562056453013.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

#### 🌹 [3] 8888 port 열기

port 여는 곳 : `# vi /etc/sysconfig/iptables`

 <center>
<figure>
<img src="/assets/post-img/linux/1562056550213.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

서버 재시작 : `# /etc/init.d/httpd stop`, `# /etc/init.d/httpd start`

방화벽 재시작 : `# /etc/init.d/iptables stop`, `# /etc/init.d/iptables start`

<br>

#### 🌹 [4] url 접속해서 확인해보기

> 기존의 80 port 
>
>  <center>
> <figure>
> <img src="/assets/post-img/linux/1562056587009.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

> django 프로젝트를 넣을 8888 port
>  <center>
> <figure>
> <img src="/assets/post-img/linux/1562056602800.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

<br>

#### ⭐ [5] JKMountFile 설정

django-톰캣 연동과 함께 할 경우 JKMountFile  설정은 

아파치 설치 경로 밑의`conf/extra/httpd-vhosts.conf`에 해야한다.

**1)  아파치 설치 경로 밑의`conf/httpd.conf` 수정**

`JkMountFile conf/uriworkermap.properties` 주석처리하기

 <center>
<figure>
<img src="/assets/post-img/linux/1562056692927.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

**2) 아파치 설치 경로 밑의`conf/extra/httpd-vhosts.conf`에 추가**

80포트의 VirtualHost 설정 안에 넣어주면 된다.

 <center>
<figure>
<img src="/assets/post-img/linux/1562056722983.png" alt="views">
<figcaption></figcaption>
</figure>
</center>



<br>

<br>










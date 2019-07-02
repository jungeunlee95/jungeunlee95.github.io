---
layout: post
title: 7 리눅스에 python 의존성 경로 설정하기(export PYTHONPATH)
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

git에서 리눅스로 [python 크롤링 프로젝트](https://github.com/jungeunlee95/python-crawler)를 clone해왔다.

> [git 에서 리눅스로 python project 가져오는 방법](https://jungeunlee95.github.io/linux/2019/06/30/5-리눅스에-파이썬-가상-개발-환경-구성하기(Virtual-Environments)/)

python-crawler에서 `__main__.py`를 실행하면 `__result__/파일이름.csv`로 저장되도록 코드를 작성했다.

`__main__.py`를 실행하기 위해서는 가상환경을 실행하고 다음 명령어를 치면 된다.

`(venv) [root@localhost python-crawler]# python3 __main__.py `

<br>

만약 프로젝트 밖에서 실행한다면,

`(venv) [root@localhost python-projects]# python python-crawler`

>  (자동으로 그 프로젝트의 `__main__.py` 실행)

<b style="color:red;">오류</b>가 발생한다. ( `__results__` 디렉토리를 찾을 수 없음)

<br>

그 이유는, `__main__.py`에서 크롤링 데이터 저장 경로를 

`table.to_csv('__results__/nene.csv', encoding='utf-8', mode='w', index=0)`로 설정했기 때문에, 프로젝트 밖에서는 실행할 수 없다. 이 경로를 <b style="color:blue;">절대 경로</b>로 바꿔 주어야 한다.

#### ⭐**python 모듈의 절대 경로 찾는 법**

`print(os.path.abspath(__file__))`

> ```
> <출력>
> D:\dowork\PycharmProjects\python_crawler\__main__.py
> ```
>
> 우리는 `__main__.py`의 상위 경로를 찾은 뒤 `__result__` 디렉토리를 찾아주어야 한다.

<br>

#### ⭐**python 상위 디렉토리 경로 찾는 법** 

`BASE_DIR = os.path.dirname(os.path.abspath(__file__))`

> <출력>
>
> D:\dowork\PycharmProjects\python_crawler

위의 크롤링 데이터 저장 경로를 다음과 같이 절대 경로로 변경하면 된다.

```python
# 모듈의 절대위치 구하기(다른 환경에서 실행할 때)
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
RESULT_DIR = f'{BASE_DIR}/__results__'
table.to_csv(f'{RESULT_DIR}/nene.csv', encoding='utf-8', mode='w', index=0)
```

하지만, 저장 파일을 프로젝트 내부에 두지 않고, 리눅스 디렉토리에 넣고 싶어 다음 경로로 변경한뒤 리눅스의 `root/`밑에 `crawling-results` 디렉토리를 만들어주어 크롤링 데이터를 저장했다.

`table.to_csv('/root/crawling-results/nene.csv', encoding='utf-8', mode='w', index=0)`  --> `/root`경로에 crawling-results 디렉토리 만들면 된다!

------

<br>

## linux에서 venv 실행을 하지 않고 python apllication 실행하기

### [1] PYTHONPATH='application 의존성 경로' 설정

1, `git clone https://github.com/jungeunlee95/python-crawler.git`

2, ` pip3 install -r python_crawler/requirements.txt --target=python_crawler/packages`

3, `export PYTHONPATH='/root/python-projects/python_crawler/packages'`

3, `python3 python_crawler` : 실행 성공!

<center>
<figure>
<img src="/assets/post-img/linux/1562054620202.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

### [2] PYTHONPATH='application 의존성 경로' 설정

1, `git clone https://github.com/jungeunlee95/python-crawler.git`

2, ` pip3 install -r python-crawler/requirements.txt --target=python-crawler`

> python 모듈을 프로젝트 바로 밑에 설치해주면, 굳이 PATH를 설정하지 않아도 된다.

3, `python3 python-crawler` : 실행 성공

<center>
<figure>
<img src="/assets/post-img/linux/1562054592169.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

<br>

### [3] 압축

1, `git clone https://github.com/jungeunlee95/python-crawler.git`

2, ` pip3 install -r python-crawler/requirements.txt --target=python-crawler`

3, `python3 -m zipapp python-crawler`

4, `python3 python-crawler.pyz` : 압축파일 실행 시 오류가 생김

> 압축했을 때, numpy 오류가 생긴다.
>
> <center>
> <figure>
> <img src="/assets/post-img/linux/1562054711369.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>
>
> 프로젝트의 pandas와 numpy관련 코드를 주석처리 하고 다시 project를 pull 받으면 오류가 사라진다.. 이 이유는 더 찾아봐야 할 것 같다.

<br>

### [4] 실행 가능 파일로 packing

1, `git clone https://github.com/jungeunlee95/python-crawler.git`

2, ` pip3 install -r python-crawler/requirements.txt --target=python-crawler`

3, `python3 -m zipapp -p "/cafe24/python3.7/bin/python3" python-crawler`

4, `python3 python-crawler.pyz`

<br>

<br>

<br>








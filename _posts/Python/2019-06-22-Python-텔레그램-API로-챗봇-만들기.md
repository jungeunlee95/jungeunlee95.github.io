---
layout: post
title: Python 텔레그램 API를 활용한 챗봇 만들기
category: Python
tags: [Python, HTTPServer]
comments: true
---

---

# Python 텔레그램 API를 활용한 챗봇 만들기

## [1]  텔레그램 가입하기

[텔레그램 홈페이지에서 가입하기](https://web.telegram.org/ )



## [2] 봇 생성 = Key 발급받기

1, 텔레그램 가입 후 `BotFather`를 검색한다.

<center>
<figure>
<img src="/assets/post-img/python/1561213651257.png" alt="views">
<figcaption></figcaption>
</figure>
</center>



2, `BotFather` 대화창에서 `/start`를 입력한다.

> 각 종 기능들을 설명과 함께 알려준다.
>
> <center>
> <figure>
> <img src="/assets/post-img/python/1561213690950.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>



3, 봇 만들기

`/ newbot` 명령어를 친 뒤, 생성할 봇의 이름을 정의해주면

KEY를 발급 받을 수 있다.

> <center>
> <figure>
> <img src="/assets/post-img/python/1561216038854.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>



4, 방금 만든 봇 이름을 검색해서 친구 추가 하기!

<center>
<figure>
<img src="/assets/post-img/python/1561216171161.png" alt="views">
<figcaption></figcaption>
</figure>
</center>



5, 메세지 보내놓기 

<center>
<figure>
<img src="/assets/post-img/python/1561216222584.png" alt="views">
<figcaption></figcaption>
</figure>
</center>





## [3] python을 통해 Bot에게 보낸 메세지 받기

1, Python을 통해 방금 위에서 Bot에게 보낸 메세지 데이터 가져오기

requests 모듈 사용을 위해서 install을 해주어야 한다. ->  `pip install requests` 

```python
import requests
import os

# 환경 변수 PATH에 등록된 TELEGRAM_TOKEN을 가져온다.
token = os.getenv('TELEGRAM_TOKEN')

# 업데이트 내용 받아오기
# 아래의 주소를 호출하면, 업데이트 된 봇의 내용을 가져올 수 있다.
url = 'https://api.telegram.org/bot{}/getUpdates'.format(token)
response = json.loads(requests.get(url).text) # json으로 받기
print(response)

'''
결과값
{'ok': True, 'result': [{'update_id': 412826971, 'message': {'message_id': 1, 'from': {'id': 748290634, 'is_bot': False, 'first_name': 'Jungjung'}, 'chat': {'id': 748290634, 'first_name': 'Jungjung', 'type': 'private'}, 'date': 1561216164, 'text': '/start', 'entities': [{'offset': 0, 'length': 6, 'type': 'bot_command'}]}}, {'update_id': 412826972, 'message': {'message_id': 2, 'from': {'id': 748290634, 'is_bot': False, 'first_name': 'Jungjung', 'language_code': 'ko'}, 'chat': {'id': 748290634, 'first_name': 'Jungjung', 'type': 'private'}, 'date': 1561216177, 'text': 'Hi !!!!!!!!!'}}]}

'''
```



## [4] Bot에게 받은 메세지 그대로 전송해보기

```python
import requests
import os

# 환경 변수 PATH에 등록된 TELEGRAM_TOKEN을 가져온다.
token = os.getenv('TELEGRAM_TOKEN')

# 업데이트 내용 받아오기
# 아래의 주소를 호출하면, 업데이트 된 봇의 내용을 가져올 수 있다.
url = 'https://api.telegram.org/bot{}/getUpdates'.format(token)
response = json.loads(requests.get(url).text) # json으로 받기

# 아래의 주소를 이용하면 Bot을 통해 메세지를 전송할 수 있다.
url = 'https://api.telegram.org/bot{}/sendMessage'.format(token)

# 나의 chat_id와, msg를 getUpdates 통해 가져온 json데이터를 파싱해서 가져온다.
chat_id = response["result"][-1]["message"]["from"]["id"]
msg = response["result"][-1]["message"]["text"]
# print(chat_id)
# print(msg)

# sendMessage url을 통해 Bot에게 parameter를 전송한다.
requests.get(url, params = {"chat_id" : chat_id, "text" : msg})
```

<center>
<figure>
<img src="/assets/post-img/python/1561216797809.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

---



## 활용하기

```python
text = response["result"][-1]["message"]["text"]
msg = text
if text == '안녕':
    msg = '안녕하세요'

requests.get(url, params = {"chat_id" : chat_id, "text" : msg})
```

<center>
<figure>
<img src="/assets/post-img/python/1561217489174.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

 text 데이터를 가져와 아래와 같이 활용하면,

텔레그램과 크롤링, api를 활용해서 원하는 정보를 편하게 확인 할 수 있다.

---

 나는 평소 방탈출 카페를 즐겨 가는데, 매번 홈페이지에서 예약 정보를 확인하기가 귀찮아서

자주가는 방탈출 카페의 예약 정보를 크롤링하여서 챗봇을 이용해 활용했다.

<center>
<figure>
<img src="/assets/post-img/python/1561217668067.png" alt="views">
<figcaption></figcaption>
</figure>
</center>





##    	[다른 텔레그램 챗봇 활용 예시 보러가기](https://jungeunlee95.github.io/python/2019/06/19/API-%ED%99%9C%EC%9A%A9-%ED%85%94%EB%A0%88%EA%B7%B8%EB%9E%A8-%EC%B1%97%EB%B4%87%EC%9C%BC%EB%A1%9C-%EC%9E%A0%EC%8B%9C%ED%9B%84-%EB%8F%84%EC%B0%A9-%EB%B2%84%EC%8A%A4-%EC%A0%95%EB%B3%B4-%EB%B0%9B%EA%B8%B0/)
---
layout: post
title: 1 JavaScript 기본 사용법, 객체 기본
category: HTML & CSS & Javascript
tags: [CSS, CSS속성]
comments: true
---

---

<center>
<figure>
<img src="/assets/post-img/htmlcss/1562137972524.png" alt="views">
<figcaption></figcaption>
</figure>
</center>

## [1] 자바스크립트 사용법

1) 본문 혹은 head안에서 작성

```html
<head>
<script type="text/javascript">
   자바스크립트 소스코드
</script>
</head>
<body>
... 
</body>
```

2) 외부 파일 불러서 실행 ( 보통 이렇게 자바스크립트를 관리한다.) 

```html
<head>
<script type="text/javascript" src="hello.js"></script>
</head>
<body>
... 
</body>
```

------

<br>

[자바스크립트 Object 이해하기 참고](<http://insanehong.kr/post/javascript-object/>)

## [2] Javascript 기본 데이터 타입

**✔ [특징]**

- Javascript의 `class free` : class가 없어도 객체 생성이 가능하다.

- 객체가 객체를 포함하기 쉬운 구조이다. (그래프, 트리, 맵 등을 쉽게 표현할  수 있다.)

- 객체 하나에 있는 속성들을 다른 객체에 상속해 주는 프로토 타입 연결 특성이 있다.

  > 초기화 시간 단축, 메모리 절약가능
  >
  > ex) 테이블 데이터가 무수히 많아지면 원페이지 처리를 어떻게 할 것인가

- Object Literal -> JSON



**✔ [1] Primitive Data Type (기본 자료형)**

- Number
- Boolean
- String
- null 
- undefined



**✔ [2]  객체**

- Array
- Date
- Math
- RegExp
- Function
- Object



**✔ [3] 유사 객체** - 기본타입도 객체와 유사한 성질을 갖고있는 것

- Number
- String
- Boolean

```js
var number = 5;
console.log(number + ' : ' + typeof(number));
```

> 5 : number 

<br>

함수는 생성자처럼 쓰는 '그냥' 함수 객체이다. 함수에 new를 다 붙일 수 있다. 

```js
var number2 = new Number(5); 
console.log(number2 + ' : ' + typeof(number2));

console.log(number + number2); 
```

> 5 : object
>
> 10

<br>

**자바스크립트 타입 확인해보기**

```js
var n = 10;
var f = 3.14;
var b = true;
var s = 'hello world';
var fn = function(){};
var o = {};
var a = [];

console.log(n + ' : ' + typeof(n));
console.log(f + ' : ' + typeof(f));
console.log(b + ' : ' + typeof(b));
console.log(s + ' : ' + typeof(s));
console.log(fn + ' : ' + typeof(fn));
console.log(o + ' : ' + typeof(o));
console.log(a + ' : ' + typeof(a));
```

> 10 : number
>
> 3.14 : number
>
> true : boolean
>
> hello world : string
>
> function(){} : function
>
> [object Object] : object
>
> : object

<br>

### - new 생성자 함수()를 사용해서 객체를 생성

```js
// new 생성자 함수()를 사용해서 객체를 생성할 때, 
var n2 = new Number(5);
var f2 = new Number(3.14);
var b2 = new Number(false);
var s2 = new String('hello world');
var fn2 = new Function('a', 'b', 'return a + b;'); 
					// 파라미터 a, b, 함수 본문
var o2 = new Object();
var a2 = new Array();
```

> new를 사용해도 '그냥' 함수객체임! 객체를 만드는 거임!
>
> 5 : object
>
> 3.14 : object
>
> 0 : object
>
> hello world : object
>
> function anonymous(a,b
>
> ) {
>
> return a + b;
>
> } : function
>
> [object Object] : object
>
> : object

<br>

### - 자바스크립트의 객체 분류

자바스크립트 객체는 3개로 분류할 수 있다.

1. Native opject (Built-in object, core object)

   > - 애플리케이션의 환경과 관계없이 언제나 사용할 수 있는 애플리케이션 전역의 공통 기능을 제공한다. 
   > - Object, String, Number, Function, Array, RegExp, Date, Math와 같은 객체 생성에 관계가 있는 함수 객체와 메소드로 구성된다.

2. 호스트 객체

   > - 브라우저 환경에서 객체를 말한다.
   > - window, XmlHttpRequest, HTMLElement 등의 DOM 노드 객체와 같이 호스트 환경에 정의된 객체
   > - 

3. 사용자 정의 객체

<br>

------

### - 내장객체, object 객체, 브라우저 내장객체 

자바스크립트의 <b style="color:blue;">내장 타입의 객체는 확장이 불가능하다.</b>

하지만, new로 함수를 이용햐서 object타입의 객체를 생성하면 가능하다. (object 타입의 객체는 확장이 가능)

```js
# 불가능
j = 10
j.abs = -10
# 가능
i = new Number(10)l
i.abs = -10
```

```js
// 내장 타입의 객체는 확장이 불가능
var n = 10;
n.myVal = 20;
console.log(n.myVal);

// object 타입의 객체는 확장이 가능
var n2 = new Number(5);
n2.myVal = 20;
console.log(n2.myVal);
```

> undefined
>
> 20

<br>

<b style="color:blue;">브라우저 내장객체 console만 알아보고 나머지는 더 공부 후에 알아보기로</b>

```js
// 브라우저 내장 객체
console.log('로그입니다.');
console.warn('경고입니다.');
console.error('에러입니다.');
console.info('정보입니다.'); 
```

> <center>
> <figure>
> <img src="/assets/post-img/htmlcss/1562137490702.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

------

### - null VS undefined 차이 알아보기

null과 undefined  모두 자바스크립트에서 값이 없음을 의미한다.

**undefined ? **  

>   undefined는 데이터 타입이자, 값을 나타낸다.
>
> 기본적으로 값이 할당되지 않으면 undefined 타입이며, 변수 값 또한 undefined이다. 정의되지 않은 것, 존재하지 않는 값이라고 볼 수 있다.

**null ?**

>null은 데이터 타입이자, 값을 나타낸다.
>
>null타입의 변수는 <b style="color:blue;">명시적</b>으로 값이 비어있음을 나타낼 때 사용한다.
>
>아무것도 참조하고 있지 않음을 나타내며 주로 객체를 담을 변수를 초기화 할 때 사용한다. 

**null VS undefined 차이점**

>undefined는 변수를 선언만 하더라도 값이 할당되지만,
>
>null은 변수를 선언한 후에 null로 값을 바꿔주어야 한다.

<br>

**실습**

```js
var myVar1;
var myVar2 = null;

console.log(myVar1 + " : " + myVar2);
console.log(typeof(myVar1) + " : " + typeof(myVar2));
console.log(myVar1 == myVar2); // 값 비교
console.log(myVar1 === myVar2); // type까지 비교
```

> undefined : null
>
> undefined : object
>
> true
>
> false

---

### - 등치성, 동일성 비교 & 형변환해보기

```js
//== (!, equality, 값의 등치성, 형변환o)
console.log("2" == 2); // true
console.log(true == 1); // true
console.log('abc' == new String('abc')); // true

//형변환
console.log(2 + "2"); // 22
console.log("2" + 2); // 22

console.log(true + 10); // 11
console.log('abc' + new String('abc')); // abcabc

console.log('========================================')
//== (!, identity, 객체의 동일성, 형변환x)
console.log("2" === 2); // fasle
console.log(true === 1); // fasle
console.log('abc' === new String('abc')); // fasle


//추천:
//== 연산자를 사용할 때는 엄격하게 형변환을 해서
//두 피연산자의 타입을 맞춘다.
str = "5";
console.log(parseInt(str) == 5); // true
```

------

### - 변수범위 알아보기

- 변수에는 무조건 var를 붙인다.

  > 1. var 사용 여부에 영향을 받는다.
  > 2. 특히 local(함수 내부)에서 var를 사용해서 변수를 정의하면 local 범위를 가진다.

```js
// 변수 범위(Scope)
var i = 5; 
var f = function(){
	var i = 20;
	console.log(i);
	i = i - 1; 
} 
f();
console.log(i);
```

> 20
>
> 5

------

### - statement 

- js engine이 읽어들여서 실행하는 단위

js는 `\n` or `;(세미콜론)`을 기준으로 단위를 읽어 실행한다.

```js
// statement
// js engine이 읽어들여서 실행하는 단위
// js는 `\n` or `;(세미콜론)`을 기준으로 단위를 읽어 실행한다.
var s = 'hello world'; console.log(s);

// 전역(global) 객체 window
// console.log(s);
console.log(window.s);
```

> hello world
>
> hello world

------

### - 전역객체(window) 확인

```js
var o = {};
o.name = '둘리';
o.age = new Number(10);
o.age.man = 9; // 만 나이

console.log(window.o);
console.log(window.o.name);
console.log(window.o.age);
```

> <center>
> <figure>
> <img src="/assets/post-img/htmlcss/1562137900976.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

```js
// json으로 작성
var o2 = {
	'name' : '둘리',
	'age' : {
		'value' : new Number(11),
		'man' : 11
	}
}
console.log(window.o2);
console.log(window.o2.name);
console.log(window.o2.age);
```

> <center>
> <figure>
> <img src="/assets/post-img/htmlcss/1562137908222.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

------

## [3] 객체 정의, 생성

```js
// 객체(type object)를 생성하는 방법 1
// new 키워드를 사용하는 방법
var o1 = new Object();
o1.name = '둘리';
o1.age = 10;
console.log(o1);

//객체(type object)를 생성하는 방법 2
// literal
var o2 = {};
o2.name = '마이콜';
o2.age = 20;
console.log(o2);

// 객체(type object)를 생성하는 방법 3
// (J)ava(S)cript (O)bject (N)otation
var o3 = {
    name : '또치',
    age : 30
}
console.log(o3);
```

> <center>
> <figure>
> <img src="/assets/post-img/htmlcss/1562137915379.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

**함수 사용**

```js
// 객체(type object)를 생성하는 방법 4
// 커스텀 생성 함수를 사용하는 방법

// 보통 일반 함수 관례 소문자 시작
var f = function(){
    console.log('보통 일반 함수');
}

// 생성자 함수 관례 : 대문자 시작
var MyObject = function(name, age){ // 객체 지향을 흉내
    console.log('생성자 함수');
    this.name = name;
    this.age = age;
}

f();
// MyObject(); // 이렇게 호출 용도가 아님 

var o4 = new f();
var o5 = new MyObject('도우넛', 40);
console.log(o4);
console.log(o5);
```

> <center>
> <figure>
> <img src="/assets/post-img/htmlcss/1562137921851.png" alt="views">
> <figcaption></figcaption>
> </figure>
> </center>

------



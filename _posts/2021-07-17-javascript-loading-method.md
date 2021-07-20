---
layout: single
title: '[Javascript] HTML에서의 자바스크립트 로딩 방법'

categories:
  - Javascript

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

HTML에서 자바스크립트를 로딩하는 몇가지 표기 방법 및 웹브라우저에서의 동작방식에 대해 알아보고자한다

### 일반적인 방법

#### head 에서 로딩

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="main.js"></script>
  </head>
</html>
```

위와같이 head 안에서 script 태그만을 사용하여 로딩하는 방식이다.  
이러한 로딩방식은 다음과 같이 동작한다.
![Image Not Found](/assets/images/javascript_loading_method_1.png)
해당 방식은 Javascript 를 다운받고 실행시키는 동안 HTML Parsing이
Block 되기에 페이지의 Draw가 지연된다는 단점이 있으며, HTML Parsing 전에 Javascript 실행이 완료됨으로
만약 HTML Tag 에 종속적인 code가 있다면 Error가 발생한다.

#### body 끝에서 로딩

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div></div>
    <script src="main.js"></script>
  </body>
</html>
```

body 끝부분에서 script 태그만을 사용하여 로딩하는 방식이다.
![Image Not Found](/assets/images/javascript_loading_method_2.png)
해당 방식은 사용자에게 웹 페이지를 빠르게 만들어 보여주고 이후에 Javascript 처리를 한다는 장점이 있지만
페이지 자체가 Javascript에 의존적이라면 사용자가 정상적으로 웹 페이지를 사용하려면 역시 시간이 소요될 수 있다는
단점이 있다.

### head 에서 async 사용

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script async src="main.js"></script>
  </head>
</html>
```

위와같이 head 안에서 script 태그에 async 속성을 추가하여 로딩하는 방식이다.  
이러한 로딩방식은 다음과 같이 동작한다.
![Image Not Found](/assets/images/javascript_loading_method_3.png)
해당 방식은 HTML parsing 중에 Javascript를 동시에 다운 받고 실행시에 Block 을 하는 방식으로
Javascipt의 다운이 병렬로 이루어지기에 조금 더 빠르다는 장점이 있지만 역시나 실행시에는 Block을 하기에
사용자가 컨텐츠를 이용하는데 시간이 소요될 수 있으며, 역시나 HTML Parsing 전에 Javascript가 실행된다는
단점 또한 존재한다.

또한 여러개의 script를 사용한다면 다운로드가 완료된 script 부터 실행하기에 실행의 순서를 보장할 수 없어, 실행순서가 중요하다면 사용해서는 안된다.
![Image Not Found](/assets/images/javascript_loading_method_4.png)

### head 에서 defer 사용

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script defer src="main.js"></script>
  </head>
</html>
```

위와같이 head 안에서 script 태그에 defer 속성을 추가하여 로딩하는 방식이다.  
이러한 로딩방식은 다음과 같이 동작한다.
![Image Not Found](/assets/images/javascript_loading_method_5.png)
해당 방식은 HTML parsing 중에 Javascript를 동시에 다운 받고 HTML Parsing이 끝나면 Javascipt를 실행시키는 방식이다.

여러개의 script를 사용한다면 페이지가 다 준비된 후 Script를 페이지에 명시된 대로 차례대로 실행시킨다.
![Image Not Found](/assets/images/javascript_loading_method_6.png)

### 활용 방안

개인적인 생각으로 해당 내용들을 정리하여 실제 활용할 방법에 대해 적어보면 다음과 같다.

1. head 에서 일반적인 방법으로 로딩하는 것은 지양하는 것이 좋을 것 같다.
2. body 끝에서 로딩하는 방법은 JS 파일이 무겁지 않고 실행 내용이 단순 하다면 사용하여도 될 것 같다.
3. async 의 경우 JS 실행 순서가 정해져 있지 않고 중간중간 먼저 실행하는것이 중요하다면 사용하여도 될 것 같다.
4. defer 는 가장 유저 친화적인 방법이라고 생각이 되는데, JS가 무거운 편이며, 실행 순서가 중요하다면 활용해서 성능향상을 도모 할 수 있을것 같다.

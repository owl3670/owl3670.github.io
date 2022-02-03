---
layout: single
title: '[GO] GO 변수와 상수'

categories:
  - GO

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

Go 언어는 변수를 선언할 때 var 키워드를 사용하여 선언한다.
선언과 동시에 값을 할당 할 수도 있으며, 선언과 동시에 값 할당 시에는 타입을 생략할 수 있다.

```go
// 변수 선언
var a int
a = 1

// 선언과 동시에 값 할당
var b int = 2

// 타입 생략
var c = 3
```

동일한 타입의 변수를 복수 개 생성한다면 변수들을 콤마(",")를 통해 나열 후 끝에서 타입을 한번만 지정하면 된다.
변수를 복수개 선언하면서 동시에 값을 할당할 수 있으며, 나열 순서대로 값이 할당된다.

```go
// 변수 복수개 선언
var a, b, c int

// 변수 복수개 선언과 동시에 값 할당
var d, e, f int = 1, 2, 3
```

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

변수 선언 시 괄호로 묶어서 나열하듯이 선언 할 수 있다. 이때 나열되는 변수들은 같은 타입일 필요는 없다.

```go
// 변수 선언 시 괄호로 묶어서 나열
var (
  a int
  b int

  // 선언과 동시에 할당 가능
  c int = 3

  // 선언과 동시에 할 당시 타입 생략 가능
  d = 4

  // 같은 괄호 내에서 다른 타입선언 가능
  e = "string"
)

a = 1
b = 2
```

Go 언어의 변수는 Short Assignment Statement (:=) 를 사용하여 var 키워드 와 타입을 생략 할 수 있으나 함수 내에서만 가능한 표현이다.
Go 언어 함수의 밖에서 모든 문(statement)은 키워드로 시작하여야 한다.

```go
// 함수 밖에서 선언하였기에 에러 발생
// syntax error: non-declaration statement outside function body
a := 1

func main(){
  // var 키워드 및 타입 생략
  b := 2

  c := "string"
}
```

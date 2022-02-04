---
layout: single
title: '[GO] GO 함수'

categories:
  - GO

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

함수란 Ou이put(결과)을 내기 위해 Input 을 받아 작동하는 프로그램 코드의 집합이다.
GO 에서 함수는 func 키워드로 정의 하며 다른 C 계열의 언어들과 다르게 리턴 타입을 가장 마지막에 기재한다. 리턴 값이 없다면 리턴 타입을 기재하지 않게 된다. 파라미터 정의시에도 타입은 뒤에 기재하게 된다. 함수를 정의후 사용시에는 함수명과 괄호안에 인자값을 넘겨주는 형태로 호출하게 된다.

```GO
package main

import "fmt"

// main 함수 정의
// Paramter, Return 값이 없으면 기재하지 않는다.
func main() {
	print(10)
	result := sum(2, 5)
	fmt.Println(result)
}

// int 값 a 를 입력받아 콘솔에 출력하는 함수.
// Return 값은 존재하지 않기에 기재하지 않는다.
func print(a int) {
	fmt.Println(a)
}

// int 값 a 와 b 를 입력받아 더한 값을 Return 하는 함수.
func sum(a int, b int) int {
	return a + b
}
```

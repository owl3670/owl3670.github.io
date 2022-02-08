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

함수란 Output(결과)을 내기 위해 Input 을 받아 작동하는 프로그램 코드의 집합이다.
GO 에서 함수는 func 키워드로 정의 하며 다른 C 계열의 언어들과 다르게 리턴 타입을 가장 마지막에 기재한다. 리턴 값이 없다면 리턴 타입을 기재하지 않게 된다. 파라미터 정의시에도 타입은 뒤에 기재하게 된다. 함수를 정의후 사용시에는 함수명과 괄호안에 인자값을 넘겨주는 형태로 호출하게 된다.

```GO
package main

import "fmt"

// main 함수 정의
// Paramter, Return 값이 없으면 기재하지 않는다.
func main() {
	print(10)
	result1 := sum(2, 5)
	fmt.Println(result1)

	result2 := multiply(2, 5)
	fmt.Println(result2)
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

// int 값 a 와 b 를 입력받아 더한 값을 Return 하는 함수.
// 파라미터의 타입이 같다면 축약해서 표현 할 수 있다.
func multiply(a, b int) int {
	return a * b
}
```

Go 언어에서 함수는 한개의 값이 아닌 여러개의 값을 리턴 할 수도 있으며, 리턴 할 변수 명을 미리 지정할 수 도 있다.
리턴 할 변수 명을 미리 지정한다면 함수안 return 지시자 뒤에 아무 인자가 없을시에 미리 지정된 변수가 자동으로 리턴된다.

```go
package main

import "fmt"

func main() {
	print(10)
	result1, result2 := multiReturn(2, 5)
	fmt.Println(result1, result2)

	result3 := namedReturn(3, 3)
	fmt.Println(result3)
}

// 두 개의 리턴 값을 가지는 함수
func multiReturn(a, b int) (int, int){
	return a + b, a * b
}

// 명명된 변수 리턴 함수
func namedReturn(a, b int)(result int){
	result = a + b
	return
}

// 여러개의 명명된 변수 리턴도 가능
func multiNamedReturn(a, b int) (sumResult int, multipleResult int){
	sumResult = a + b
	multipleResult = a * b
	return
}
```

함수의 인자를 가변적으로 만들 수 있으며 이때는 '...'을 타입명 앞에 표기하여 사용 가능하다.

```go
package main

import "fmt"

func main() {
	multiArgFunc1(1, 2, 3)
	multiArgFunc1(1, 2, 3, 4, 5)

	multiArgFunc2(1, 2, 3, 4)
}

// 가변인자 함수
func multiArgFunc1(nums ...int) {
	fmt.Println(nums)
}

//  가장뒤에 가변인자를 표시 하고 앞에는 순서대로 입력받을 다른 파라미터를 정의할 수 있다.
func multiArgFunc2(num int, nums ...int){
	fmt.Println(num, nums)
}
```

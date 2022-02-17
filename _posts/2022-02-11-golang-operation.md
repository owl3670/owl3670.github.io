---
layout: single
title: '[GO] GO 연산자'

categories:
  - GO

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

### 산술 연산자

```go
package main

import "fmt"

func main(){
  a, b := 10, 3

  fmt.Println("a + b = ", a+b)  // 더하기 연산자 +
  fmt.Println("a - b = ", a-b)  // 빼기 연산자 -
  fmt.Println("a * b = ", a+b)  // 곱하기 연산자 *
  fmt.Println("a / b = ", a+b)  // 나누기 연산자 /
  fmt.Println("a % b = ", a%b)  // 나머지 연산자 %
}
```

### 증감 연산자

```go
package main

import "fmt"

func main(){
  a, b := 1, 5

  a++
  b--
  // Go 언어에서는 증감 연산자를 표현식으로 사용 할 수 없기에 에러 발생
  // syntax error: unexpected ++ at end of statement
  var c = a++

  fmt.Println("a++ : ", a)
  fmt.Println("b-- : ", b)
  // Go 언어에서는 증감 연산자를 표현식으로 사용 할 수 없기에 에러 발생
  // syntax error: unexpected ++ at end of statement
  fmt.Println("a++ : ", a++)
  // syntax error: unexpected --, expecting comma or )
  fmt.Println("b++ : ", b--)
}
```

### 비트 연산자

```go
package main

import "fmt"

func main(){
  a := 21 // 0001 0101
  b := 15 // 0000 1111

  fmt.Printf("a & b : %d %08b\n", a&b, a&b)  // bit AND 연산자              0000 0101
  fmt.Printf("a | b : %d %08b\n", a|b, a|b)  // bit OR 연산자               0001 1111
  fmt.Printf("a ^ b : %d %08b\n", a^b, a^b)  // bit XOR 연산자              0001 1010
  fmt.Printf("a &^ b : %d %08b\n", a&^b, a&^b)  // bit AND NOT 연산자       0001 0000
  fmt.Printf("a << 2 : %d %08b\n", a << 2, a << 2)  // bit 좌측 shift 연산자 0101 0100
  fmt.Printf("a >> 2 : %d %08b\n", a >> 2, a >> 2)  // bit 우측 shift 연산자 0000 0101
}
```

### 할당 연산자

```go
package main

import "fmt"

func main(){
  var a = 21 // 일반 할당 연산자
  b := 15 // 축약된 할당 연산자

  a += 1
  fmt.Println("a += 1 : ", a) // 22
  a -= 1
  fmt.Println("a -= 1 : ", a) // 21
  a *= 2
  fmt.Println("a *= 2 : ", a) // 42
  a /= 2
  fmt.Println("a /= 2 : ", a) // 21
  a %= 2
  fmt.Println("a %= 2 : ", a) // 1
  a &= 2
  fmt.Println("a &= 2 : ", a) // 0
  a |= 2
  fmt.Println("a |= 2 : ", a) // 2
  a ^= 2
  fmt.Println("a ^= 2 : ", a) // 0
  a &^= 2
  fmt.Println("a &^= 2 : ", a) // 0
  b <<= 2
  fmt.Println("b <<= 2 : ", b) // 60
  b >>= 2
  fmt.Println("b >>= 2 : ", b) // 15
}
```

### 관계 연산자

```go
package main

import "fmt"

func main(){
  a, b, c := 10, 15 ,10

  fmt.Println("a == b : ", a == b)  // false
  fmt.Println("a == c : ", a == c)  // true
  fmt.Println("a != b : ", a != b)  // true
  fmt.Println("a != c : ", a != c)  // false
  fmt.Println("a < b : ", a < b) // true
  fmt.Println("a < c : ", a < c) // flase
  fmt.Println("a > b : ", a > b)  // false
  fmt.Println("a > c : ", a > c)  // false
  fmt.Println("a <= b : ", a <= b) // true
  fmt.Println("a <= c : ", a <= c) // true
  fmt.Println("a >= b : ", a >= b)  // false
  fmt.Println("a >= c : ", a >= c)  // true
}
```

### 논리 연산자

```go
package main

import "fmt"

func main(){
 a, b := true, false

 fmt.Println("a && a : ", a && a) // true
 fmt.Println("a && b : ", a && b) // fasle
 fmt.Println("b && b : ", b && b) // false
 fmt.Println("a || a : ", a || a) // true
 fmt.Println("a || b : ", a || b) // true
 fmt.Println("b || b : ", b || b) // false
 fmt.Println("!a : ", !a) // false
 fmt.Println("!b : ", !b) //true
}
```

### 포인터 연산자

```go
package main

import "fmt"

func main(){
 a := 10
 b := &a // a 의 메모리 주소 참조 (& 연산자)

 fmt.Println("a : ", a) // 10
 fmt.Println("b : ", b) // a 의 메모리 주소
 fmt.Println("*b : ", *b) // b 포인터가 가르키는 주소에 저장된 값 참조 (* 연산자) 10

 *b += 5
 fmt.Println("a : ", a) // 15
 fmt.Println("*b : ", *b) // 15

 // Go 언어는 포인터 산술연산을 지원하지 않으므로 에러 발생
 // invalid operation: b++ (non-numeric type *int)
 b++
 // invalid operation: b += 2 (mismatched types *int and int)
 b += 2
}
```

### 채널 연산자

```go
package main

import (
	"fmt"
)

func main(){
 a := make(chan int) // 채널 생성

 go func(){
   b, c := 5, 10
   a <- b + c // 채널로 데이터 송신 (<- 연산자)
 }()

 result := <- a // 채널로 부터 결과값 수신 (<- 연산자)
 fmt.println(result) // 15
}
```

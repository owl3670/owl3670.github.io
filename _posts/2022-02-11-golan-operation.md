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

### 비트 연산자

```go
package main

import "fmt"

func main(){
  a := 21 // 0001 0101
  b := 15 // 0000 1111

  fmt.Printf("a & b : %d %08b\n", a&b, a&b)  // bit AND 연산자
  fmt.Printf("a | b : %d %08b\n", a|b, a|b)  // bit OR 연산자
  fmt.Printf("a ^ b : %d %08b\n", a^b, a^b)  // bit XOR 연산자
  fmt.Printf("a &^ b : %d %08b\n", a&^b, a&^b)  // bit AND NOT 연산자
  fmt.Printf("a << 2 : %d %08b\n", a << 2, a << 2)  // bit 좌측 shift 연산자
  fmt.Printf("a >> 2 : %d %08b\n", a >> 2, a >> 2)  // bit 우측 shift 연산자
}
```

### 관계 연산자

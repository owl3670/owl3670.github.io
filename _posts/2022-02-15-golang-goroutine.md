---
layout: single
title: '[GO] GO루틴'

categories:
  - GO

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

Go루틴 (goroutine) 은 Go runtime 에 의해 관리되는 경량화된 스레드로 다른 함수나 메소드와 동시에 실행되는 함수이다.
새로운 Go루틴을 실행하려면 함수 또는 메소드 호출 시 go 키워드를 앞에 붙이면 된다.

```go
package main

import (
	"fmt"
	"time"
)

func print(s string) {
	time.Sleep(100 * time.Millisecond)
	fmt.Println(s)
}

func main() {
	fmt.Println("Function Start")
	print("Doing Something")
	fmt.Println("Function Finished")

	fmt.Println("Function Start")
	go print("Doing Something")
	fmt.Println("Function Finished")
}

```

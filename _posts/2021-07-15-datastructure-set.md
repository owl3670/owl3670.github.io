---
layout: single
title: '[자료구조] Set'

categories:
  - 자료구조

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

Set 은 유니크한 값들을 순서가 없이 순열 형태로 저장 할 수 있는 추상화된 자료구조이다.

### 특징

- 순서를 보장하지 않는다(Unorderd)
- 고유한 값을 가지고 있도록 요소 삽입 시 중복 값 존재여부를 확인한다
- 주로 해당 데이터가 존재하는지 여부를 빠르게 확인하기 위해 사용한다

### Set 내부 동작

HashTable 형태로 구현하는 경우가 많은 것 같지만 일부 언어(C++)등에서는 이진트리 형태로 구현하는 경우도 존재하는 듯 하다.
이 글에서는 HashTable 형태를 예시로 한다.

- 추가하는 요소의 Hash 값을 구해온다
- Hash 값을 HashTable 에 저장한다
- HashTable 형태로 값을 저장하기에 값 탐색시 O(1)의 성능이 기대된다

---
layout: single
title: "[프로그래머스][Python] 정수 삼각형"

categories:
 - Programmers

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

난이도 : 프로그래머스 Level 3  
문제 모음 : 코팅테스트 고득점 Kit 동적계획법

### 문제 설명
![Image Not found](/assets/images/programmers_int_triangle.png)  
위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

### 제한사항
 * 삼각형의 높이는 1 이상 500 이하입니다.
 * 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

### 입출력 예  
  
| triangle | result |    
|---|---|  
|[[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]] | 30 |  

### 나의 풀이
삼각형의 아랫줄에서 부터 계산을 해나가며 결과를 합쳐나가는 방식을 사용하였는데 삼각형내의 각 요소는 각각 왼쪽 아래와 오른쪽 아래의 요소와 더하게 되고 더한 값들 중에 큰 수를 결과로 선택한다.  
최종적으로 꼭대기의 요소가 정답이 된다.  

```python
def solution(triangle):
    for idx1 in range(len(triangle) - 1, 0, -1):
        for idx2 in range(len(triangle[idx1 - 1])):
            left = triangle[idx1][idx2] + triangle[idx1 - 1][idx2]
            right = triangle[idx1][idx2 + 1] + triangle[idx1 - 1][idx2]
            triangle[idx1 - 1][idx2] = max(left, right)
    
    return triangle[0][0]
```

### 다른 풀이
다른 사람의 풀이중 분석해볼 코드를 가져왔다. 
```python
solution = lambda t, l = []: max(l) if not t else solution(t[1:], [max(x,y)+z for x,y,z in zip([0]+l, l+[0], t[0])])
```
람다와 list slice, list comprehension, zip, 재귀함수 개념등이 들어가서 한줄 코드로 끝난 풀이인데 나의 풀이랑은 반대로 삼각형의 꼭대기에서 부터 계산을 해나가는 방식이다.  
계산의 편의를 위해 연산 결과값의 list 처음과 끝에 각각 0을 붙여놓고 zip 과 list comprehension으로 결과 값의 list를 점차 만들어 가는 것이 너무 멋진 코드인 것 같다.

### 문제 출처
* [프로그래머스 코딩테스트 연습](https://programmers.co.kr/learn/courses/30/parts/12263){: target="_blank"}
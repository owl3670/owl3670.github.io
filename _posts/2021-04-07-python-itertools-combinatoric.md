---
layout: single
title: "[Python] itertools 조합형"

categories:
 - Python

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

### 개요
Python의 표준 라이브러리중 효율적인 반복작업을 위한 함수를 모은 itertools 모듈에는 조합을 위한 함수들이 존재하는데 이를 자세히 알아보려 한다.

### 종류

|함수|설명|  
|---|---|  
|product(*args, repeat=1)|cartesian product (곱집합)|  
|permutations(iterable, r=None)|길이 r의 튜플들이며, 반복되는 요소 없이 모든 가능한 조합들이다|  
|combinations(iterable, r)|길이 r의 튜플들이며, 반복되는 요소 없이 정렬된 순서의 조합들이다|  
|combinations_with_replacement(iterable, r)|길의 r의 튜플들이며, 반복되는 요소가 있는 정렬된 순서의 조합들이다|  

### 사용예시
* 코드

```python
import itertools

product_result = list(itertools.product([1,2,3,4], repeat=2)) #product([1,2,3,4], [1,2,3,4]) 와 같다.
permutations_result = list(itertools.permutations([1,2,3,4], 2))
combinations_result = list(itertools.combinations([1,2,3,4], 2))
combinations_with_replacement_result = list(itertools.combinations_with_replacement([1,2,3,4],  2))

print(f'product : {product_result}')
print(f'permutations : {permutations_result}')
print(f'combinations : {combinations_result}')
print(f'combinations_with_replacement : {combinations_with_replacement_result}')
```
* 결과

product : [(1, 1), (1, 2), (1, 3), (1, 4), (2, 1), (2, 2), (2, 3), (2, 4), (3, 1), (3, 2), (3, 3), (3, 4), (4, 1), (4, 2), (4, 3), (4, 4)]  
permutations : [(1, 2), (1, 3), (1, 4), (2, 1), (2, 3), (2, 4), (3, 1), (3, 2), (3, 4), (4, 1), (4, 2), (4, 3)]  
combinations : [(1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]  
combinations_with_replacement : [(1, 1), (1, 2), (1, 3), (1, 4), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (4, 4)]

### 더 알아보기
파이썬 공식 문서를 통해 각 함수가 어떻게 구현되어있는지 자세히 보고자 한다.  
* product

입력이 정렬되어 있다면, 결과가 정렬된 순서로 방출된다.
실제로 구현되어 있는 코드에서는 메모리에 중간 결과를 쌓지 않는다는 점을 제외하고 다음 코드와 동등하다고 한다.

```python
#예시 product([1,2,3], repeat=2)
def product(*args, repeat=1):
    pools = [tuple(pool) for pool in args] * repeat 
    #pools : [(1, 2, 3), (1, 2, 3)]
    result = [[]]
    for pool in pools:
        result = [x+[y] for x in result for y in pool]
        #loop 1 result : [[1],[2],[3]]
        #loop 2 result : [[1, 1], [1, 2], [1, 3], [2, 1], [2, 2], [2, 3], [3, 1], [3, 2], [3, 3]]
    for prod in result:
        yield tuple(prod)
```
* permutations

입력이 정렬되어 있다면, 결과가 정렬된 순서로 방출된다.
입력에서 중복된 값이 없다면, 결과에서 반복 값은 없다.
다음 코드와 동등하다고 한다.

```python
#예시 permutations([1,2,3])
def permutations(iterable, r=None):
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    if r > n:
        return
    indices = list(range(n))
    cycles = list(range(n, n-r, -1))
    yield tuple(pool[i] for i in indices[:r])
    #indices : [0, 1, 2], yield : (1, 2, 3)
    while n:
        for i in reversed(range(r)):
            cycles[i] -= 1
            #loop 1 cycles[2] : 0, loop 2 cycles[1] : 1, loop 3 cycles[2] : 0
            #loop 4 cycles[1] : 0, loop 5 cycles[0] : 2, loop 6 cycles[2] : 0
            #loop 7 cycles[1] : 1, loop 8 cycles[2] : 0, loop 9 cycles[1] : 0
            #loop 10 cycles[0] : 1, loop 11 cycles[2] : 0, loop 12 cycles[1] : 1
            #loop 13 cycles[2] : 0, loop 14 cycles[1] : 0, loop 15 cycles[0] : 0
            if cycles[i] == 0:
                indices[i:] = indices[i+1:] + indices[i:i+1]
                cycles[i] = n - i
                #loop 1 i : 2, indices : [0, 1, 2], cycles : [3, 2, 1]
                #loop 3 i : 2, indices : [0, 2, 1], cycles : [3, 1, 1]
                #loop 4 i : 1, indices : [0, 1, 2], cycles : [3, 2, 1]
                #loop 6 i : 2, indices : [1, 0, 2], cycles : [2, 2, 1]
                #loop 8 i : 2, indices : [1, 2, 0], cycles : [2, 1, 1]
                #loop 9 i : 1, indices : [1, 0, 2], cycles : [2, 2, 1]
                #loop 11 i : 2, indices : [2, 0, 1], cycles : [1, 2, 1]
                #loop 13 i : 2, indices : [2, 1, 0], cycles : [1, 1, 1]
                #loop 14 i : 1, indices : [2, 0, 1], cycles : [1, 2, 1]
                #loop 15 i : 0, indices : [0, 1, 2], cycles : [3, 2, 1]
            else:
                j = cycles[i]
                indices[i], indices[-j] = indices[-j], indices[i]
                yield tuple(pool[i] for i in indices[:r])
                #loop 2 i : 1, j : 1, indices : [0, 2, 1], yield : (1, 3, 2)
                #loop 5 i : 0, j : 2, indices : [1, 0, 2], yield : (2, 1, 3)
                #loop 7 i : 1, j : 1, indices : [1, 2, 0], yield : (2, 3, 1)
                #loop 10 i : 0, j : 1, indices : [2, 0, 1], yield : (3, 1, 2)
                #loop 12 i : 1, j : 1, indices : [2, 1, 0], yield : (3, 2, 1)
                break
        else:
            return
```

product 결과에서 반복값을 걸러낸 후 인덱스로 사용하는 형태로 구현될수도 있다.

```python
#예시 permutations([1,2,3])
def permutations(iterable, r=None):
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    for indices in product(range(n), repeat=r):
        #loop 1 indices :(0, 0, 0), loop 2 indices :(0, 0, 1), loop 3 indices :(0, 0, 2)
        #loop 4 indices :(0, 1, 0), loop 5 indices :(0, 1, 1), loop 6 indices :(0, 1, 2)
        #loop 7 indices :(0, 2, 0), loop 8 indices :(0, 2, 1), loop 9 indices :(0, 2, 2)
        #loop 10 indices :(1, 0, 0), loop 11 indices :(1, 0, 1), loop 12 indices :(1, 0, 2)
        #loop 13 indices :(1, 1, 0), loop 14 indices :(1, 1, 1), loop 15 indices :(1, 1, 2)
        #loop 16 indices :(1, 2, 0), loop 17 indices :(1, 2, 1), loop 18 indices :(1, 2, 2)
        #loop 19 indices :(2, 0, 0), loop 20 indices :(2, 0, 1), loop 21 indices :(2, 0, 2)
        #loop 22 indices :(2, 1, 0), loop 23 indices :(2, 1, 1), loop 24 indices :(2, 1, 2)
        #loop 25 indices :(2, 2, 0), loop 26 indices :(2, 2, 1), loop 27 indices :(2, 2, 2)
        if len(set(indices)) == r:
            yield tuple(pool[i] for i in indices)
            #loop 6 indices :(0, 1, 2), yield : (1, 2, 3)
            #loop 8 indices :(0, 2, 1), yield : (1, 3, 2)
            #loop 12 indices :(1, 0, 2), yield : (2, 1, 3)
            #loop 16 indices :(1, 2, 0), yield : (2, 3, 1)
            #loop 20 indices :(2, 0, 1), yield : (3, 1, 2)
            #loop 22 indices :(2, 1, 0), yield : (3, 2, 1)
```
* combinations

입력이 정렬되어 있다면, 결과가 정렬된 순서로 방출된다.
입력에서 중복된 값이 없다면, 결과에서 반복 값은 없다.
다음 코드와 동등하다고 한다.

```python
#예시 combinations([1,2,3], r=2)
def combinations(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    if r > n:
        return
    indices = list(range(r))
    yield tuple(pool[i] for i in indices)
    #indices : [0, 1], yield : (1, 2)
    while True:
        for i in reversed(range(r)):
            #loop 1-1 indices[1] : 1
            #loop 2-1 indices[1] : 2
            #loop 2-2 indices[0] : 0
            #loop 3-1 indices[1] : 2
            #loop 3-2 indices[0] : 1
            if indices[i] != i + n - r:
                break
        else:
            return
        indices[i] += 1
        for j in range(i+1, r):
            indices[j] = indices[j-1] + 1
            #loop 2-1 j : 1, indices : [1, 2]
        yield tuple(pool[i] for i in indices)
        #loop 1 indices : [0, 2], yield : (1, 3)
        #loop 2 indices : [1, 2], yield : (2, 3)
```
permutaion 결과를 정렬된 형태만 걸러내어 인덱스로 사용하는 방법으로 구현 할 수도 있다.

```python
def combinations(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    for indices in permutations(range(n), r):
        #loop 1 indices : (0, 1)
        #loop 2 indices : (0, 2)
        #loop 3 indices : (1, 0)
        #loop 4 indices : (1, 2)
        #loop 5 indices : (2, 0)
        #loop 6 indices : (2, 1)
        if sorted(indices) == list(indices):
            yield tuple(pool[i] for i in indices)
            #loop 1 indices : (0, 1), yield : (1, 2)
            #loop 2 indices : (0, 2), yield : (1, 3)
            #loop 4 indices : (1, 2), yield : (2, 3)
```
* combinations_with_replacement

입력이 정렬되어 있다면, 결과가 정렬된 순서로 방출된다.
입력에서 중복된 값이 없다면, 결과에서 반복 값은 없다.
다음 코드와 동등하다고 한다.

```python
def combinations_with_replacement(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    if not n and r:
        return
    indices = [0] * r
    yield tuple(pool[i] for i in indices)
    #indices : [0, 0], yield : (1, 1)
    while True:
        for i in reversed(range(r)):
            #loop 1-1 indices[1] : 0
            #loop 2-1 indices[1] : 1
            #loop 3-1 indices[1] : 2
            #loop 3-2 indices[0] : 0
            #loop 4-1 indices[1] : 1
            #loop 5-1 indices[1] : 2
            #loop 5-2 indices[0] : 1
            #loop 6-1 indices[1] : 2
            #loop 6-2 indices[0] : 2
            if indices[i] != n - 1:
                break
        else:
            return
        indices[i:] = [indices[i] + 1] * (r - i)
        yield tuple(pool[i] for i in indices)
        #loop 1 indices : [0, 1], yield : (1, 2)
        #loop 2 indices : [0, 2], yield : (1, 3)
        #loop 3 indices : [1, 1], yield : (2, 2)
        #loop 4 indices : [1, 2], yield : (2, 3)
        #loop 5 indices : [2, 2], yield : (3, 3)
```
product 결과를 정렬된 형태만 걸러내어 인덱스로 사용하는 방법으로 구현 할 수도 있다.

```python
def combinations_with_replacement(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    for indices in product(range(n), repeat=r):
        #loop 1 indices : (0, 0)
        #loop 2 indices : (0, 1)
        #loop 3 indices : (0, 2)
        #loop 4 indices : (1, 0)
        #loop 5 indices : (1, 1)
        #loop 6 indices : (1, 2)
        #loop 7 indices : (2, 0)
        #loop 8 indices : (2, 1)
        #loop 9 indices : (2, 2)
        if sorted(indices) == list(indices):
            yield tuple(pool[i] for i in indices)
            #loop 1 indices : (0, 0), yield : (1, 1)
            #loop 2 indices : (0, 1), yield : (1, 2)
            #loop 3 indices : (0, 2), yield : (1, 3)
            #loop 5 indices : (1, 1), yield : (2, 2)
            #loop 6 indices : (1, 2), yield : (2, 3)
            #loop 9 indices : (2, 2), yield : (3, 3)
```

### 참고 자료
* <https://docs.python.org/ko/3/library/itertools.html>{: target="_blank"}
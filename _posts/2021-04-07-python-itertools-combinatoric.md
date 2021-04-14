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
각 함수가 어떻게 구현되어있는지 자세히 보고자 한다.  
* product

```python
def product(*args, repeat=1):
    pools = [tuple(pool) for pool in args] * repeat
    result = [[]]
    for pool in pools:
        result = [x+[y] for x in result for y in pool]
    for prod in result:
        yield tuple(prod)
```
* permutations

```python
def permutations(iterable, r=None):
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    if r > n:
        return
    indices = list(range(n))
    cycles = list(range(n, n-r, -1))
    yield tuple(pool[i] for i in indices[:r])
    while n:
        for i in reversed(range(r)):
            cycles[i] -= 1
            if cycles[i] == 0:
                indices[i:] = indices[i+1:] + indices[i:i+1]
                cycles[i] = n - i
            else:
                j = cycles[i]
                indices[i], indices[-j] = indices[-j], indices[i]
                yield tuple(pool[i] for i in indices[:r])
                break
        else:
            return
```
```python
def permutations(iterable, r=None):
    pool = tuple(iterable)
    n = len(pool)
    r = n if r is None else r
    for indices in product(range(n), repeat=r):
        if len(set(indices)) == r:
            yield tuple(pool[i] for i in indices)
```
* combinations

```python
def combinations(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    if r > n:
        return
    indices = list(range(r))
    yield tuple(pool[i] for i in indices)
    while True:
        for i in reversed(range(r)):
            if indices[i] != i + n - r:
                break
        else:
            return
        indices[i] += 1
        for j in range(i+1, r):
            indices[j] = indices[j-1] + 1
        yield tuple(pool[i] for i in indices)
```
```python
def combinations(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    for indices in permutations(range(n), r):
        if sorted(indices) == list(indices):
            yield tuple(pool[i] for i in indices)
```
* combinations_with_replacement

```python
def combinations_with_replacement(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    if not n and r:
        return
    indices = [0] * r
    yield tuple(pool[i] for i in indices)
    while True:
        for i in reversed(range(r)):
            if indices[i] != n - 1:
                break
        else:
            return
        indices[i:] = [indices[i] + 1] * (r - i)
        yield tuple(pool[i] for i in indices)
```
```python
def combinations_with_replacement(iterable, r):
    pool = tuple(iterable)
    n = len(pool)
    for indices in product(range(n), repeat=r):
        if sorted(indices) == list(indices):
            yield tuple(pool[i] for i in indices)
```
---
layout: single
title: "[프로그래머스][Python] 완주하지 못한 선수"

categories:
 - Programmers

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---
 
난이도 : 프로그래머스 Level 1

### 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

### 제한사항
 * 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
 * completion의 길이는 participant의 길이보다 1 작습니다.
 * 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
 * 참가자 중에는 동명이인이 있을 수 있습니다.

### 입출력 예  
  
| participant | completion | return |  
|---|---|---|  
| ["leo", "kiki", "eden"] | ["eden", "kiki"] | "leo" |  
| [“marina”, “josipa”, “nikola”, “vinko”, “filipa”] | [“josipa”, “filipa”, “marina”, “nikola”] | “vinko” |
| [“mislav”, “stanko”, “mislav”, “ana”] | [“stanko”, “ana”, “mislav”] | “mislav” |

### 입출력 예 설명
예제 #1 "leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #2 "vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

예제 #3 "mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

### 나의 풀이
문제 조건중 단 한 명의 선수만이 마라톤을 완주하지 못했다는 것에서 생각을 출발하였는데,
주어진 입력 participant, completion 을 각각 정렬 후 앞에서 부터 같은 index의 요소끼리 순차적으로 비교시 
completion 에 없는 한 명의 선수를 찾아낼 수 있다는 생각으로 다음과 같이 코드를 작성했다.

```python
def solution(participant, completion):
    answer = ''
    
    participant.sort()
    completion.sort()        
    
    answer = participant[-1]
    
    for i in range(len(completion)):
        if(participant[i] != completion[i]):
            answer = participant[i]
            break;
    
    return answer
```
completion의 길이만큼 비교를 하더라도 다른요소가 없을 시에는 participant의 마지막이 완주하지 못한 선수이기에 answer 변수에 participant의 마지막 요소를 담고 루프를 돌렸다.

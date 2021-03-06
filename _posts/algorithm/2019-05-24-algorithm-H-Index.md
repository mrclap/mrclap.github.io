---
layout: post
title: (정렬)H-Index, 완성한 for문도 다시 살피자
categories:
  - algorithm
# feature_image: ""
---
### 프로그래머스의 H-Index문제
>
H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과1에 따르면, H-Index는 다음과 같이 구합니다.
어떤 과학자가 발표한 논문 n편 중, h번 이상 인용된 논문이 h편 이상이고 나머지 논문이 h번 이하 인용되었다면 h가 이 과학자의 H-Index입니다.
어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

```python
def solution(citations):
    answer = 0
    citations.sort(reverse=True)
    for idx, citation in enumerate(citations):
        if idx+1 == citation:
            answer = idx+1
            break
        elif idx+1 > citation:
            answer = idx
            break
        answer = len(citations)
    return answer
```

정렬과 for문을 통해 if조건을 검색하면 간단히 해결할 수 있는 문제였다.
논문을 내림차순으로 정렬해서 index+1과 해당 index의 인용 수를 비교하는 방식으로 답을 구했다.
다만, `index+1 == citation`인 경우는 곧 바로 index+1이 H-Index값이지만, `index+1 > citation`의 경우는 **그 직전 index**가 원하는 target index이므로 조건절이 2개로(case는 총 3개) 나눠졌다.

문제를 풀고나면 확인할 수 있는(아래) 다른 사람들의 풀이의 for문과 비교하니 나의 for문이 초라해 보인다.. 덕지 덕지 if elif에 break까지...

인용된 수가 큰 것부터 차례로 check하지말고, 인용된 수가 작은 것부터 check하면 위 풀이의 2개 조건을 하나로 합칠 수 있다. 그리고 이를 통해서 좀 더 짧고 간결한 코드를 만들 수 있다!

```python
def solution(citations):
    citations.sort()
    l = len(citations)
    for i in range(l):
        if citations[i] >= l-i:
            return l-i
    return 0
```

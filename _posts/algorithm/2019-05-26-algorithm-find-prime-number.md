---
layout: post
title: (완전탐색)모의고사, list의 index통일에 유의하자!
categories:
  - algorithm
# feature_image: ""
---
### 프로그래머스의 모의고사 문제
> 1번 수포자가 찍는 방식: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, …  
2번 수포자가 찍는 방식: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3, 2, 4, 2, 5, …    
3번 수포자가 찍는 방식: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, …    
1번 문제부터 마지막 문제까지의 정답이 순서대로 들은 배열 answers가 주어졌을 때, 가장 많은 문제를 맞힌 사람이 누구인지 배열에 담아 return 하도록 solution 함수를 작성해주세요. 수포자는 수학을 포기한 사람의 준말입니다. 수포자 삼인방은 모의고사에 수학 문제를 전부 찍으려 합니다. 수포자는 1번 문제부터 마지막 문제까지 다음과 같이 찍습니다. 제한 조건 시험은 최대 10,000 문제로 구성되어있습니다. 문제의 정답은 1, 2, 3, 4, 5중 하나입니다. 가장 높은 점수를 받은 사람이 여럿일 경우, return하는 값을 오름차순 정렬해주세요.

```python
'''
2차 눈에 보이는 깔끔하지 못한 부분 수정 ver
- 학생을 기준으로 patterns/cnts의 index를 통일
'''
def solution(answers):
    answer = []

    patterns = [[1,2,3,4,5], [2,1,2,3,2,4,2,5], [3,3,1,1,2,2,4,4,5,5]]
    cnts = [0,0,0]

    for x in range(len(answers)):
        for y in range(len(patterns)):
            if answers[x] == patterns[y][x%len(patterns[y])]:
                cnts[y] += 1
        # if answers[x] == patterns[0][x%len(patterns[0])]:
        #     cnts[0] += 1
        # if answers[x] == patterns[1][x%len(patterns[1])]:
        #     cnts[1] += 1
        # if answers[x] == patterns[2][x%len(patterns[2])]:
        #     cnts[2] += 1

    cnt_max = max(cnts)

    for x in range(len(patterns)):
        if cnt_max == cnts[x]:
            answer.append(x+1)

    return answer



'''
5/26 1차 통과

cnt리스트라던가 몇 가지 부분이 마음에 들지 않는다..
동작만하는 코드 정도라고 해야하나?
'''
def solution(answers):
    answer = []

    pattern_1 = [1,2,3,4,5]
    pattern_2 = [2,1,2,3,2,4,2,5]
    pattern_3 = [3,3,1,1,2,2,4,4,5,5]

    cnt_1 = 0
    cnt_2 = 0
    cnt_3 = 0

    for x in range(len(answers)):
        if answers[x] == pattern_1[x % len(pattern_1)]:
            cnt_1 += 1
        if answers[x] == pattern_2[x % len(pattern_2)]:
            cnt_2 += 1
        if answers[x] == pattern_3[x % len(pattern_3)]:
            cnt_3 += 1
        cnt = [cnt_1, cnt_2, cnt_3]

    cnt_max = max(cnt)

    for x in range(len(cnt)):
        if cnt_max == cnt[x]:
            answer.append(x+1)

    return answer
```

웃픈(?) 문제다.. 전체문제를 찍는다니...

각 학생의 찍는 패턴을 list로 만들어서 실제 답을 for문으로 순회하며 각 학생의 찍는 패턴과 비교하면 되는 문제다. 처음의 풀이를 보면 학생1, 2, 3을 의도적으로 **변수명 뒤에 underscore를 사용해서 구분**하는 **비효율**을 보여준다.

각 학생이 list들(pattern, cnt)의 index를 하나씩 자치하면 굳이 변수명으로 구분하지 않아도 될 뿐더러 **for문을 이용해서 중복을 ~~최소화~~ 줄일** 수 있다.

**중복을 줄이기 위해서, 동일한 대상과 관련있는 list를 운영할때는 index를 적극 활용하자!!**

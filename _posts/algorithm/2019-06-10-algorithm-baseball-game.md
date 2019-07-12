---
layout: post
title: (완전탐색)숫자 야구 게임, 다시 한 번 모든 경우의 조합을 만들어내기
categories:
  - algorithm
# feature_image: ""
---
### 프로그래머스의 숫자 야구 게임 문제
> 각자 서로 다른 1~9까지 3자리 임의의 숫자를 정한 뒤 서로에게 3자리의 숫자를 불러서 결과를 확인합니다. 그리고 그 결과를 토대로 상대가 정한 숫자를 예상한 뒤 맞힙니다. 
> * 숫자는 맞지만, 위치가 틀렸을 때는 볼  
* 숫자와 위치가 모두 맞을 때는 스트라이크  
* 숫자와 위치가 모두 틀렸을 때는 아웃  
A : 123  
B : 1스트라이크 1볼.  
A : 356  
B : 1스트라이크 0볼.  
A : 327  
B : 2스트라이크 0볼.  
A : 489  
B : 0스트라이크 1볼.  
이때 가능한 답은 324와 328 두 가지입니다.  
질문한 세 자리의 수, 스트라이크의 수, 볼의 수를 담은 2차원 배열 baseball이 매개변수로 주어질 때, 가능한 답의 개수를 return 하도록 solution 함수를 작성해주세요.


```python
'''
6/10 1차 시도
만들 수 있는 모든 경우의 숫자조합을 만들어 놓고(1~9의 숫자를 중복하지 않고 순서와 상관있이 3개 뽑음)
각 시도마다 불가능한 녀석들을 지워가며 후보들을 줄이는 방식으로 풀이.
'''
CONSTANT = dict()
CONSTANT['size'] = 3


def solution(baseball):
    answer = 0

    candidates = baseball_number_set_maker()
    candidates_dict = {str(candidate): 1 for candidate in candidates}

    IDX_TRIAL = 0
    IDX_STRIKE = 1
    IDX_BALL = 2

    while(baseball):
        this_turn = baseball.pop(0)

        trial = str(this_turn[IDX_TRIAL])
        guess_strike = this_turn[IDX_STRIKE]
        guess_ball = this_turn[IDX_BALL]

        for candidate in candidates_dict:
            strike = 0
            ball = 0
            if candidates_dict[candidate] == 1:
                for idx in range(len(trial)):
                    if trial[idx] == candidate[idx]:
                        strike += 1
                    for jdx in range(len(this_turn)):
                        if trial[idx] == candidate[jdx]:
                            ball += 1
                ball -= strike
                if guess_ball == ball and guess_strike == strike:
                    pass
                else:
                    candidates_dict[candidate] = 0
            else:
                pass

    for candidate in candidates_dict:
        if candidates_dict[candidate] == 1:
            answer += 1
    return answer


def baseball_number_set_maker():
    NUMBERS = [x for x in range(1, 10)]
    number_set = list()
    str = ''
    attaching_number(str, NUMBERS, number_set)

    return number_set


def attaching_number(making, number_remain, number_set):
    if len(making) == CONSTANT['size']:
        number_set.append(int(making))
        return
    else:
        for idx in range(0, len(number_remain)):
            new_making = making + str(number_remain[idx])
            new_number_remain = number_remain.copy()
            del new_number_remain[idx]
            attaching_number(new_making, new_number_remain, number_set)


if __name__ == '__main__':
    baseball = [[123, 1, 1], [356, 1, 0], [327, 2, 0], [489, 0, 1]]  # 2
    print(solution(baseball))
```

'itertool'을 한 번 사용해보겠다고 다짐하고는 다시 직접 전체 조합을 recursive로 만들어냈다.  
깔끔하게 만들지도 못하면서.. 왜 굳이 직접 만들었을까..  
쓸데없는 상수(대문자)와 적합하지 못한 변수명 그리고 난잡한 for문으로 끔찍한 모습으로 문제를 푼 것 같다.

- 굳이 왜 recursive를 사용해서 모든 조합을 만들지 않아도 아래처럼. 멋지게 모든 경우의 조합을 만들어 낼 수 있다.  
(다른 분의 코드!)
```python
  for i in range(111,1000):

        ch = str(i)
        if ch[0] == ch[1] or ch[0] == ch[2] or ch[1] == ch[2]:
            continue
        if ch[0] == '0' or ch[1] == '0' or ch[2] == '0':
            continue
```
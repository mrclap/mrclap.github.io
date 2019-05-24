---
layout: post
title: 짧고 이해하기 어려운 코드와 길고 이해하기 쉬운 코드에 대하여
categories:
  - diary
# feature_image: ""
tags:
---
>**짧고 이해하기 쉬운 코드가 정답이다.**


[programmers에서 아주 간단한 정렬 문제](https://programmers.co.kr/learn/courses/30/lessons/42748)를 풀고는 들었던 생각이다.
아주 간단한 정렬문제였던 탓에, 생각의 흐름대로 작성하고 보니 약 10줄 가량 되었다.
```python
def solution(array, commands):
    answer = []
    for command in commands:
        s = command[0]
        e = command[1]
        idx = command[2]

        _array = array[s-1:e]
        _array.sort()
        answer.append(_array[idx-1])

    return answer
```
답안을 제출하고 나면 다른 사람의 답안을 볼 수 있는데, **단 두 줄**로 끝을 낸 사람들이 아주 많았다.
코드를 써내려가다보면 항상

- 함수명, 변수명을 결정하거나,
- 값을 변수에 저장하여 사용할지 혹은 그냥 바로 사용할지

등 에 고민을 하게되는데, 아주 잠깐의 시간을 들여 다른 사람들의 생각을 찾아보았다.

Quora라는 커뮤니티에 '[Is it better to write long, readable code or short, not so readable code in Python?](https://www.quora.com/Is-it-better-to-write-long-readable-code-or-short-not-so-readable-code-in-Python'라는 제목으로 올라왔던 질문의 답을 읽으니 참재미있다.

'That was a strange question! Short and readable is obviously better than long and obscure.'라고 한다.. 무릎을 탁 치게 만든다. 생각의 흐름(?)대로 코드를 짠다고 불필요하게 모든 값에 변수를 만들어 대입하고, line by line으로 작성하는 것이 꼭 **읽기 쉬운** 코드를 짜는 것이 아니라는 것이다.

더욱이 1번만 사용하며, 심지어 이번 문제와 같이 어렵지 않은 문제는 나중에 다시 코드를 살피거나, 동작중에 잘못 설계한 부분을 수정할 일이 적으므로, **굳이** 가독성을 위해 추가적인 시간을 할애할 필요가 없겠다.

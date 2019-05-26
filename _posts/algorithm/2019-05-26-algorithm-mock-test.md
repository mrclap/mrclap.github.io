---
layout: post
title: (완전탐색)소수찾기, 모든 경우의 조합을 만들어내기, recursive!
categories:
  - algorithm
# feature_image: ""
---
### 프로그래머스의 소수찾기 문제
> 한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다. 각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.    
>    
> 제한사항    
numbers는 길이 1 이상 7 이하인 문자열입니다.    
numbers는 0~9까지 숫자만으로 이루어져 있습니다.    
013은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

```python
'''
5/26 1차 시도 pass

- 집중이 안되서 오래걸렸다.
- itertool이라는 라이브러리가 있는줄 몰랐다..
- isPrime함수가 무언가 깔끔하지 못한 느낌이든다..

'''
import math


def solution(numbers):
    numbers = [x for x in numbers]
    results = []
    answer = 0

    for each in numbers:
        if each != '0':
            _numbers = numbers.copy()
            str = each
            _numbers.remove(each)
            results.append(str)
            recursive(str, _numbers, results)

    results = set([int(x) for x in results])

    for result in results:
        if is_prime(result):
            answer += 1

    return answer


def recursive(previous, numbers, results):
    for each in numbers:
        _numbers = numbers.copy()
        current = previous + each
        _numbers.remove(each)
        results.append(current)
        recursive(current, _numbers, results)


def is_prime(n):
    if n <= 1:
        return False

    START = 2
    # 1. 2~ floor(root(n)) => z 까지 범위의 list or dict 생성
    # 2. for loop으로 1. 숫자를 순회하면서
    # 3. 해당 loop의 수(d)가 n을 나눌 수 있는지 ( % d == 0)판별
    # 4. 나눠지지 않으면 해당 수의 배수 중 z보다 작은 수를 모두 소거
    candidates_dict = {x: 1 for x in range(START, math.floor(math.sqrt(n))+1)}
    for x in candidates_dict:
        if candidates_dict[x] == 1:
            if n % x != 0:
                candidates_dict[x] = 0
            set_0_for_all_multiples(x, candidates_dict)
    prime_numbers = []

    for x in candidates_dict:
        if candidates_dict[x] == 1:
            prime_numbers.append(x)

    result = True if len(prime_numbers) == 0 else False
    return result


def set_0_for_all_multiples(n, dict):
    for x in range(n*2, len(dict)+2, n):
        if dict[x] == 1:
            dict[x] = 0
```

일요일이라 그런지 집중이 너무 안돼서 오래 잡고 있었던 문제다 ~~반성하자...~~.    
주어진 1) 숫자들(1자리 수) 들을 조합하여 2) 각 조합들이 소수인지 판단하여 총 개수를 구하는 문제였다.    
`set_0_for_all_multiples`라는 함수를 이용해서 이미 약수가 아닌 수의 배수가 되는 수*(앞에서 이미 2로 나누어지지 않았음을 알았다면 2의 배수인 4, 6, 8 등의 수도 모두 나눠질 수 없다)*들의 % 연산을 건너뛰도록 했는데, 어느정도 효율이 있는지는 검증을 해보지 못했다.    
- 딕셔너리의 value가 숫자 1인지 if 검증을 하는 것 vs % 연산하여 결과가 0인지 검증을 하는 것
- 딕셔너리 운용을 위해 필요한 메모리 할당.. 등 

다시 생각해보면 그리 효율적이지 않은 것도 같은데 빼는게 나을지 모르겠다.    
***작성하면서 생각이 난 것인데, 굳이 제곱근 수 까지 다 순회하지 않아도 약수가 하나라도 발견되면 바로 `return False`를 반환하는게 훨씬 효율적이겠다.....***

`itertool`이라는 라이브러리를 통해 주어진 리스트로 모든 경우의 조합을 만들어 낼 수 있는 것 같은데, 한 번 사용해보되 당분간은 직접 구현해서 사용해야겠다.
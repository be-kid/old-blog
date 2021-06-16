---
layout: post
title:  "[210616] 백트래킹 공부 Python"
date:   2021-06-16
excerpt: "자료구조 알고리즘 공부"
tag:
- python 
- 백준
- 15649번
- 15650번
- 15651번
- 15652번
- n과 m
- 백트래킹
comments: false
---

# [Python] 백트래킹 공부하기(2)

유명한 n과 m 시리즈를 풀어보며 감을 익혔다

1~4번만 풀어봤는데 대충 어떤 느낌인지는 알 것 같다

백트래킹은 경우의 수를 구하는 알고리즘이지만

단순하게 모든 경우의 수를 다 구하는 것이 아니고 

가능한, 유효한 경우만 알뜰하게 챙기는 알고리즘이다

그렇다면 경우의 수 중에서 어떻게 유효한 내용만 골라낼 것인지가 중요하다

이를 구현해내는데 이해해야하는것 중 가장 중요한 것은 ‘상태’인 것 같다

이 '상태'라는 단어가 백트래킹을 공부를 하면서 본 내용들 중 이 알고리즘을 이해하는데 가장 도움이 된 단어였다

내가 이해한 바를 정리를 해보자면...

재귀를 통해 호출된 또 다른 자기 자신들이 모두 각각 하나의 상태가 된다

n과 m 문제를 예로 든다면

1, 2, 3, 4, 5 중에서 3개를 중복 없이 골라야 하는 상황에서

만약 어떤 단계에서 1과 2를 골랐고 나머지 3, 4, 5 중에서 하나를 골라야하는 차례라면

현재 호출된 그 함수는 1과 2를 고른 상태라고 할 수 있는 것이다 

글로 쓰려니 굉장히 어렵다

단계별로 적어봐야겠다

우선 처음에는 1 ~ 5 중에서 아무것도 고르지 않은 상태다

이 상태를 상태-0 이라 부르기로 하고

상태-0 에서 1을 고르면서 새로 자기 자신을 호출한다

새롭게 호출된 1이 골라진 녀석은 상태-1

상태-1 은 이미 고른 1을 제외한 2 ~ 4 중에서 하나를 고르고

새로운 상태인 상태-1-x 로 넘어간다 x는 당연히 2 ~ 4 중 하나다

상태-1-x 는 1과 x 를 제외한 나머지 3개의 숫자 중 하나를 고르고

상태-1-x-y 로 넘어간다

이제는 3개를 다 골랐으니 고른 3개를 출력하고 상태-1-x 로 되돌아 가야한다

상태-1-x 가 모든 경우를 다 출력하고 나면 다시 상태-1 로 갈 것이고

상태-1 도 모든 경우를 출력하고 나면 상태-0 으로 가서 상태-2로 넘어가게 된다

그리고 계속 반복...

아래는 n과 m 시리즈 1 ~ 4 번

[https://www.acmicpc.net/problem/15649](https://www.acmicpc.net/problem/15649)

[https://www.acmicpc.net/problem/15650](https://www.acmicpc.net/problem/15650)

[https://www.acmicpc.net/problem/15651](https://www.acmicpc.net/problem/15651)

[https://www.acmicpc.net/problem/15652](https://www.acmicpc.net/problem/15652)

```python
from sys import stdin

n,m = map(int,stdin.readline().split())

arr=[0 for i in range(m)]
check=[0,0,0,0,0,0,0,0,0] # 각 상태에서의 숫자 사용 여부 체크
def func(x):
  if x==m:  # m개를 다 골랐으니 출력한다
    for i in range(m):
      print(arr[i],end=' ')
    print()
    return
  else:     # 아직 m개를 못 골랐으니
    for i in range(1,n+1): # 1부터 n까지 중에서
      if check[i]==0:   # 사용하지 않은 숫자를 찾아서
        arr[x]=i        # 집어넣고
        check[i]=1      # 사용했다고 체크하고
        func(x+1)       # 고른 개수 보내기
        check[i]=0      # 돌아왔을 땐 사용했던거 다시 취소
    
func(0)

# 백트래킹 n과 m 1번
# 1부터 n까지의 수 중에서 m개의 수를 중복 없이 고르기
# 재귀로 한단계 들어갈 때 마다 새로운 상태가 생겨난다
# 각 상태마다 check 리스트가 적용되어야 하므로
# 그 상태가 끝나고 돌아왔을 때 check 리스트를 원래대로 돌린다
```

```python
from sys import stdin

n,m = map(int,stdin.readline().split())

#1부터 n까지 수 중에서 중복 없이 m개, 오름차순

check = [0,0,0,0,0,0,0,0,0] # n의 범위 1~8
ans = [0 for i in range(m)] # m개

def func(x, p):
  if x==m:
    for i in ans:
      print(i,end=' ')
    print()
    return
  else:
    for i in range(p, n+1):
      if check[i]==0:
        ans[x]=i
        check[i]=1
        func(x+1, i)
        check[i]=0
    
func(0, 1)

# n과 m 1번과는 다르게
# 오름차순이라는 조건이 존재한다
# 자연스래 중복되는 내용은 사라진다
# 다음으로 들어오는 숫자를 탐색할 때
# 마지막으로 고른 숫자보다 더 큰
# 숫자만 골라야하므로
# p라는 인수를 하나 더 생성해서
# 마지막으로 고른 숫자를
# 함께 다음 상태로 보내주었다
```

```python
from sys import stdin

n,m = map(int, stdin.readline().split())

# 같은 수를 여러번 골라도 된다

check=[0 for i in range(8)]
ans = [0 for i in range(m)]

def func(x):
  if x==m:
    for i in ans:
      print(i,end=' ')
    print()
  else:
    for i in range(1,n+1):
      if check[i]!=m:
        check[i]+=1
        ans[x]=i
        func(x+1)
        check[i]-=1
  
  
func(0)

# 같은 수도 중복이 된다
# 그 얘기는 같은 수가 m번 나올 수 있다
# check를 m까지 해서 해결
```

```python
from sys import stdin

n,m = map(int, stdin.readline().split())

check = [0 for i in range(n+1)]
ans = [0 for i in range(m)]

def func(x, p):
  if x==m:
    for i in ans:
      print(i, end=' ')
    print()
    return
  else:
    for i in range(p,n+1):
      if check[i]<m:
        check[i]+=1
        ans[x]=i
        func(x+1, i)
        check[i]-=1

func(0,1)

# 중복 선택이 되지만 오름차순이어야 한다
# 2, 3번 혼합
```
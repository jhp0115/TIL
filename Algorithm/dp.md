# Dynamic Programming
(작성 중)  
Dynamic Programming의 '예전 결과값을 저장해 연산 중복을 피한다'는 핵심 개념은 (사실상) 주인공같은 조연에 가까운 것 같다.  
DP 알고리즘 문제의 진짜 주인공은 점화식 세우기라고 생각하는 것이 더 좋을 것 같다.

## Dynamic Programming(동적 계획법)이란
- 어떤 문제를 풀기 위해 이전 문제의 결괏값이 필요할 때 유용하게 사용된다.
- DP 테이블을 이용하여 이전 계산 결과들을 저장하고 다음 문제를 풀 때 이전 계산 결과를 다시 계산하지 않고 메모해둔 것에서 꺼내서 사용한다.
- 사실 DP 테이블과 같은 결괏값 저장소를 사용하는 것도 중요하지만, DP 알고리즘 문제에서 정말 중요한 것은 귀납적으로 점화식을 잘 세우는 것이다.  

## 점화식 세우기
> ### 참고 자료
> - https://cotak.tistory.com/51  
>   - https://www.geeksforgeeks.org/solve-dynamic-programming-problem/  
> - https://www.acmicpc.net/board/view/1488

- 어려우면, 일단 초항부터 몇 가지 예시를 들어본 후, 점화식을 발견해본다.  

1. DP 문제인지 확인하기  
   - 특정 양을 최대화 또는 최소화하는 문제
      - 일반적으로 브루트 포스, 그리디로 최대화 또는 최소화를 구하려고 하면 수많은 반례에 부딛히기 쉽다고 한다. 
   - 특정 조건 또는 특정 확률 문제에서 배열을 계산해야 하는 문제
   - 피보나치 수열(f(n) = f(n-1) + f(n-2)) 문제처럼, 중복된 하위 문제가 있다.

2. 상태 결정하기  
상태는 변수에 대응되는 의미로 볼 수 있을 것 같다. 

3. 점화식 세우기  

4. Memoization or Tabulation  



## 문제 풀이
### [BOJ 1463] 1로 만들기, Silver III  
#### 정답 코드  
```python
n = int(input())

times = [0] * (1000000 + 1)

times[1] = 0
times[2] = 1
times[3] = 1

for i in range(4, n + 1):
    candidates = []
    if i % 3 == 0:
        candidates.append(times[i // 3] + 1)
    if i % 2 == 0:
        candidates.append(times[i // 2] + 1)
    candidates.append(times[i - 1] + 1)
    times[i] = min(candidates)

print(times[n])

```      
<br>
  
### [BOJ 2293] 동전 1, Gold V  

#### 정답 코드
```python
from sys import stdin

n, k = map(int, stdin.readline().rstrip().split())
coins = []
for i in range(n):
    coins.append(int(stdin.readline().rstrip()))

times = [0] * (10000 + 1)
times[0] = 1

for coin in coins:
    for i in range(coin, k + 1):
        times[i] += times[i - coin]

print(times[k])
```

#### 후기
이 문제를 통해 많은 것을 얻을 수 있었다.  
그리고 글을 통해 설명하는 것이 얼마나 어려운 것인지도 알 수 있었다..;;  
처음 제출 시 이중 for문을 만들었을 때는 아래 코드처럼 안쪽 for문과 바깥 for문을 정답과 반대로 작성했었다.
```python
# 틀린 코드: 이렇게 하면 순서만 다른 경우도 카운트된다.
for i in range(coin, k + 1):
    for coin in coins:
        times[i] += times[i - coin]
```  
그런데 이렇게 작성하면, 순서만 다른 경우들도 카운트되어 문제의 조건에 맞지 않게 된다.  
그럼 화폐 종류를 바깥 for문에 작성하는 것과 안쪽 for문에 작성하는 것은 어떤 차이가 있을까?  
> 일단 화폐 종류를 바깥 for문에 작성하면 다음과 같은 효과가 있다.
>- 화폐를 한 종류씩 계산하여 순서만 다른 경우가 중복으로 포함되지 않는다.
>- 예를 들어 화폐 종류가 1원, 2원, 5원이 있다고 했을 때, 1원 화폐부터 계산한다고 해보자.
>- 1원으로 0원부터 목표 금액까지 계산하고 나서, 이번에는 2원 화폐로 0원부터 목표 금액까지 계산할 때, 일종의 오름차순 정렬된 배열 형태로 2원짜리 화폐가 뒤에 append되는 느낌을 받을 수 있다. `[1, 1, 1, 1] + [2]` 또는 `[2, 2] + [2]`같은 경우이다. 절대 `[2, 1, 2] + [1]`처럼 화폐 단위의 크기가 뒤섞이는 일은 일어나지 않으므로, 중복이 생기지 않는다.

> 반면 화폐 단위 교체를 안쪽 for문에 작성하면, 화폐 단위의 크기가 뒤섞이는 예시가 나타나 순서만 다른 중복이 포함된다. 각 목표 금액마다 모든 화폐 단위를 무조건 거치기 때문이다.

정의역 중심으로 점화식을 세울 것인지, 치역 중심으로 점화식을 세울 것인지를 인식해두면 좋을 것 같다.





<br>

### [BOJ 2294] 동전 2, Gold V  
#### 정답 코드  
```python
from sys import stdin

n, k = map(int, stdin.readline().rstrip().split())
coins = []
for i in range(n):
    coins.append(int(stdin.readline().rstrip()))

nums = [0] * 10001

# 동전의 개수가 최소가 되도록. 순서만 다른 것은 같은 경우.
# 출력 목표: 사용한 동전의 최소 개수.

for i in range(min(coins), k + 1):
    candidates = []
    for coin in coins:
        if i - coin >= 0:
            if nums[i - coin] != 0:
                candidates.append(nums[i - coin] + 1)
            else:
                if i in coins:
                    candidates.append(1)
    if candidates:
        nums[i] = min(candidates)

if nums[k] != 0:
    print(nums[k])
else:
    print(-1)
```
- nums 배열의 숫자를 10001로 초기화한 후 candidates배열을 사용하지 않고 min()함수를 이용하는 방법도 있다.
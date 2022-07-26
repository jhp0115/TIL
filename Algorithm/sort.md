# 정렬

## 정렬 알고리즘의 종류
- 선택 정렬
- 삽입 정렬
- 퀵 정렬
- 계수 정렬
- 기타(나중에 추가 공부)

## 선택 정렬
배열에서 가장 작은 데이터를 선택하여 0번 원소와 바꾸고, 그 다음으로 작은 데이터를 선택하여 1번 원소와 바꾸고, 이 과정을 계속 반복하는 정렬 방식이다.  
예를 들어 다음과 같은 배열을 선택 정렬한다고 해보자.  
`[2, 5, 1, 3, 4]`  
1. 0~4번 원소 중 가장 작은 원소를 찾아서 0번 원소와 바꾼다. 1이 가장 작으므로 1과 2의 자리를 바꾼다.  
→ `[1, 5, 2, 3, 4]`
2. 1~4번 원소 중 가장 작은 원소를 찾아서 1번 원소와 바꾼다. 2가 가장 작으므로 5와 2의 자리를 바꾼다.  
→ `[1, 2, 5, 3, 4]`
3. 2~4번 원소 중 가장 작은 원소를 찾아서 2번 원소와 바꾼다. 3이 가장 작으므로 3과 5를 바꾼다.  
→ `[1, 2, 3, 5, 4]`
4. 3~4번 원소 중 가장 작은 원소를 찾아서 3번 원소와 바꾼다. 4가 가장 작으므로 4와 5를 바꾼다.  
→ `[1, 2, 3, 4, 5]`  

이 알고리즘을 python 코드로 작성해보면 다음과 같다.
```python
array = [2, 5, 1, 3, 4]

for i in range(len(array) - 1):
    index_of_min = array.index(min(array[i:len(array)]))
    array[i], array[index_of_min] = array[index_of_min], array[i]

print(array)  # [1, 2, 3, 4, 5]
```

가장 작은 데이터를 선택하는 과정을 0번 원소부터 n-2번 원소까지 반복하므로 시간복잡도는 O(n^2)이다.


## 삽입 정렬
두 번째 원소에서부터 원소의 크기를 판단해서, 지금 원소 앞의 원소들 사이에 적절히 삽입하여 정렬하는 방식이다.  
예를 들어, 다음 배열이 있다고 해보자.  
`[3, 6, 5, 1, 2]`
이 배열을 삽입 정렬로 오름차순 정렬해보자.
1. 이 배열(리스트)에서 1번 원소 6을 0번 원소인 3의 앞으로 옮길지 아니면 뒤에 둘지(그대로 둘지) 결정한다. 오름차순 정렬을 위해 그대로 둔다.  
→ `[3, 6, 5, 1, 2]`
2. 그 다음 2번 원소 5를 0번 원소 앞, 뒤, 1번 원소 뒤 세 곳 중 어느 곳에 둘지 결정한다. 5는 3보다 크고 6보다 작으므로 0번 원소 뒤로 옮긴다.  
→ `[3, 5, 6, 1, 2]`
3. 3번 원소 1도 마찬가지로, 어느 원소 사이로 옮길 것인지 결정한다. 1은 3보다 작으므로 0번 원소 앞으로 옮긴다.  
→ `[1, 3, 6, 5, 2]`
4. 마지막 4번 원소 2도, 어느 원소 사이로 옮길 것인지 결정한다. 2는 1보다 크고 3보다 작으므로 1번 원소 앞으로 옮긴다.  
→ `[1, 2, 3, 5, 6]`  

python 코드로 작성해보면 다음과 같다.
```python
array = [3, 6, 5, 1, 2]

for i in range(1, len(array)):
    for j in range(i, 1 - 1, -1):
        if array[j - 1] > array[j]:
            array[j - 1], array[j] = array[j], array[j - 1]
        else:
            break

print(array)  # [1, 2, 3, 5, 6]
```  
이 코드에서는 인접한 원소를 반복적으로 swap하면서, 멀리 떨어진 곳에 원소를 삽입하는 것과 같은 효과를 만들었다.  

삽입 정렬의 시간복잡도도 O(n^2)이다. 다만 삽입 정렬은 필요한 경우에만 원소를 옮기기 때문에 입력받은 데이터가 이미 거의 정렬되어 있는 상태일 때 다른 정렬 알고리즘보다 속도가 더 빠르다.


## 퀵 정렬
### 알고리즘
`작성 중`

### 예시 코드
```python
array: list[int] = [3, 6, 1, 3, 2, 0, 7, 6, 11, 5]

def hoare_quick_sort(array: list[int], start: int, end: int):
    # 재귀 종료 조건.
    if end <= start:
        return

    # 분할.
    pivot = start  # 리스트 내 범위의 첫 번째 원소를 피봇으로 정한다.
    left, right = start + 1, end
    while left <= right:
        while left <= end and array[left] <= array[pivot]:
            left += 1
        # right는 pivot과 교체될 수 있기 때문에 반드시 start 이상이어야 한다.
        while right > start and array[right] >= array[pivot]:
            right -= 1
        
        # (right < left)가 참이라는 것은
        # 1. left가 리스트의 범위 밖으로 벗어났을 때와 
        # 2. right와 left가 리스트 범위 내에서 엇갈렸을 때를 모두 포함한다.
        # 1번의 경우, right는 end에서 시작해서 왼쪽으로 이동하므로 반드시 right < left이게 된다.
        if right < left:
            # cf: left가 범위 밖으로 벗어났든 아니든, right는 pivot보다 작거나 pivot 자기 자신이다.
            array[pivot], array[right] = array[right], array[pivot]
            pivot = right
        else:
           array[left], array[right] = array[right], array[left]

    # 재귀 호출.
    hoare_quick_sort(array, start, pivot - 1)
    hoare_quick_sort(array, pivot + 1, end)
    

hoare_quick_sort(array, 0, len(array) - 1)
print('result: ', array)
```

## 계수 정렬
계수 정렬은 데이터의 크기와 범위가 제한적일 때 사용할 수 있는 정렬 방식이다. 예를 들어 학생들의 성적을 정렬할 때처럼 0-100이라는 정수처럼 전체 가능한 원소의 범위가 제한적이고, 점수가 정수여서 원소의 정의역(?)이 무한하지 않을 때 사용가능한 방식이다.  
계수 정렬은 입력받은 배열을 계속 수정하면서 정렬하는 것이 아닌, 특별한 배열을 하나 만들어서 그 배열의 원래 원소의 개수를 보관한다.
예를 들어 다음과 같은 성적 정보를 입력 받았다고 해보자. (예시를 편하게 들기 위해 0-10점짜리 시험이었다고 가정한다.)
```python
# 학생의 id는 인덱스 번호로 한다.
scores = [7, 8, 6, 4, 6, 9, 8, 6]
```
이러한 성적 배열이 있을 때, 각 성적의 개수를 보관하는 별도의 배열을 다음과 같이 만들 수 있다.
```python
# 성적 값의 정의역이 0부터 10까지의 정수로 총 11개이므로.
times = [0] * 11
```
그리고 이 times 배열에, 입력받은 배열의 각 원소 값을 인덱스 번호로 하여, scores 배열의 해당 인덱스 번호의 숫자를 1씩 증가시킨다. 그러면 times 배열은 다음과 같을 것이다.
```python
times = [0, 0, 0, 0, 1, 0, 3, 1, 2, 1, 0]
```
times 배열의 인덱스 번호가 성적 값이었고, 각 원소의 값은 해당 성적의 개수를 의미했으므로, 다음과 같은 코드를 이용하면 최종 정렬된 배열을 만들 수 있다.
```python
result = []
for i in range(11):
    for j in range(times[i]):
        result.append(i)
```
그러면 결과는 `[4, 6, 6, 6, 7, 8, 8, 9]`로 잘 정렬된 것을 볼 수 있다.  
계수 정렬은 잘 사용하면 다른 정렬 방식보다 더 빠르게 정렬을 할 수 있다는 장점이 있지만, 제약 조건이 많이 붙는다는 단점이 있다.  
계수 정렬을 사용할 수 있는 조건(데이터 값들의 크기와 범위가 한정되어 있음)을 잘 기억하고 활용할 수 있어야 할 것이다.  

## 문제 유형과 활용
나동빈님의 이코테 177p 참고
>- 정렬 라이브러리로 풀 수 있는 문제
>   - python에는 퀵정렬과 병합정렬을 효율적으로 사용한 좋은 정렬 라이브러리인 timsort가 내장되어 있다. 파이썬 코드로 직접 정렬 알고리즘을 작성하는 것보다 C로 구현된 내장 라이브러리를 사용하는 것이 처리 속도가 더 빠르고 코드도 짧다.
>- 정렬 알고리즘의 원리를 알아야 풀 수 있는 문제
>   - 선택 정렬, 삽입 정렬, 퀵 정렬 등의 원리를 알고 있어야 풀 수 있는 문제
>- 더 빠른 정렬이 필요한 문제
>   - 계수 정렬 등의 다른 알고리즘을 이용하거나, 기존 알고리즘의 구조적인 개선을 거쳐야 풀리는 문제
# 이진 탐색

* 찾으려는 데이터와 중간점 위치의 데이터를 반복적으로 비교해서 원하는 데이터의 위치(인덱스)를 찾는 방법
* (시작점, 끝점, 중간점) 변수 세 개와 (찾을 데이터) 변수 하나.
* 리스트 상의 데이터는 오름차순 또는 내림차순으로 정렬되어 있어야 한다.
* 한 회 탐색할 때마다, 그 다음 탐색해야 할 데이터의 개수가 반씩 줄어들므로 시간복잡도는 O(logN)이다.

## 알고리즘
* '시작점'과 '끝점', '중간점'은 리스트의 인덱스이다.
1. 시작점와 끝점을 확인해서 중간점을 정한다.
2. `중간점의 원소값`과 `찾을 데이터(target)`를 비교하여 다음을 수행한다.
	1. `중간점의 원소값` == `찾을 데이터`:
		1. 중간점(인덱스)을 반환하고 알고리즘을 종료한다.
	2. `중간점의 원소값` < `찾을 데이터`:
		1. `시작점`을 `중간점 한 칸 다음`으로 옮긴다.
	3. `찾을 데이터` < `중간점의 원소값`:
		1. `끝점`을 `중간점 한 칸 이전`으로 옮긴다.
	4. 리스트 상에서 끝점이 시작점보다 왼쪽에 있다면, target 데이터가 리스트에 존재하지 않음을 보인 후 알고리즘을 종료하고, 그렇지 않다면  `2-1` ~ `2-4`를 반복한다.

## 예시 코드
```python
# 재귀 함수로 구현.
def binary_search(array: list, start: int, end: int, target: int):
    if end < start:
        return None

    mid: int = (start + end) // 2
    if array[mid] == target:
        return mid
    elif array[mid] < target:
        return binary_search(array, mid + 1, end, target)
    else:
        return binary_search(array, start, mid - 1, target)


array: list = [0, 2, 3, 5, 7, 8, 9, 11]
print(binary_search(array, 0, len(array) - 1, 9))  # 6
print(binary_search(array, 0, len(array) - 1, 5))  # 3
print(binary_search(array, 0, len(array) - 1, 2))  # 1
print(binary_search(array, 0, len(array) - 1, 3000))  # None
```

```python
# 반복문으로 구현.
def binary_search(array: list, start: int, end: int, target: int):
    while start <= end:
        mid: int = (start + end) // 2
        if array[mid] == target:
            return mid
        elif array[mid] < target:
            start = mid + 1
        else:
            end = mid - 1
    return None


array: list = [0, 2, 3, 5, 7, 8, 9, 11]
print(binary_search(array, 0, len(array) - 1, 9))  # 6
print(binary_search(array, 0, len(array) - 1, 5))  # 3
print(binary_search(array, 0, len(array) - 1, 2))  # 1
print(binary_search(array, 0, len(array) - 1, 2000))  # None
```

## 이진 탐색 트리
* 파일시스템, 데이터베이스 등 대용량 데이터를 관리할 때 사용하기 용이한 자료구조
* `왼쪽 자식 노드의 값` < `부모 노드의 값` < `오른쪽 자식 노드의 값`
* 새로운 값은 단말로 들어가게 된다.

## ETC
* 이진 탐색 문제는, 데이터의 개수가 아주 많이(1000만 개 이상) 주어지거나 탐색 범위의 크기가 1000억 이상을 큰 크기를 가지는 경우가 많다고 한다.
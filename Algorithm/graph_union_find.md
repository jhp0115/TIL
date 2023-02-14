# 그래프와 서로소 집합

## 서로소 집합 (Union Find)
- 그래프 상의 각 노드를 같은 집합으로 만들거나, 같은 집합에 속해있는지 확인하는 알고리즘이다.  
- Union 연산: 두 노드의 부모를 같도록 만든다. 이때, 기존 부모 인덱스가 더 큰 노드의 부모 노드 값을 수정한다.
- Find 연산: 해당 노드의 부모(주로 최상위 부모)를 반환하도록 만든다.
- 활용
  - 집합(Set) 연산 구현
  - 그래프의 사이클 여부 판단
  - 최소 신장 트리
  - 위상 정렬 알고리즘 (방향그래프에서 노드들을 순서대로 나열하기)

### 코드
```python
# parent_table: 부모 테이블. 인덱스는 노드 번호, 값은 부모의 노드 번호
parent_table: list = [i for i in range(10)]  

# union 연산. 두 노드의 부모 노드를 같게 만든다.
def union(a: int, b: int, parent_table: list):
    parent_a: int = find_parent(a, parent_table)
    parent_b: int = find_parent(b, parent_table)

    if parent_a < parent_b:
        parent_table[b] = parent_a
    else:
        parent_table[a] = parent_b


# find 연산: 인자로 들어간 노드의 부모 노드, 즉 해당 집합을 대표하는 루트 노드를 반환한다.
def find_parent(n: int, parent_table: list):
    parent: int = parent_table[n]
    while parent != parent_table[parent]:
        parent = parent_table[parent]
    return parent

```

## 최소 신장 트리(MST), 크루스칼 알고리즘
- 최소 신장 트리: 주어진 그래프에서 사이클 없이 최소한의 간선으로 모든 노드를 연결시킨 부분 그래프
- 크루스칼 알고리즘
    > - 각 간선을 비용이 적은 순으로 방문하면서 다음을 수행한다.  
        - 해당 간선이 연결하는 두 노드가 같은 집합에 속한다면, 해당 간선은 MST에 포함시키지 않는다.  
        - 두 노드가 같은 집합이 아니라면, 해당 간선을 MST에 포함시키고 두 노드를 같은 집합에 속하게 한다.
    

## 위상 정렬 알고리즘(Topology sort)
- 방향 그래프에서, 모든 노드를 순서를 거스르지 않고 순서대로 나열하는 알고리즘
- 알고리즘
    > - 진입차수가 0인 노드를 큐에 넣는다.
    > - 큐가 빌 때까지 다음을 반복한다.
        - 큐에서 원소를 꺼내서 해당 노드에서 시작하는 간선을 그래프에서 제거한다.
        - 새롭게 진입차수가 0이 된 노드를 큐에 넣는다.
- 사이클이 있다면 모든 노드를 방문할 수 없다. 알고리즘 실행 도중에 진입차수가 0인 노드가 없어지기 때문이다.
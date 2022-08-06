# 최단 경로 알고리즘

## 다익스트라(Dijkstra) 최단 경로 알고리즘
- 음의 간선이 없을 때 사용한다.
- 한 노드에서부터 다른 모든 노드까지의 최단 거리를 구하는 알고리즘이다.

### 알고리즘
>1. 최단 거리를 저장할 배열 distance의 원소를 INF로 초기화한다.
>2. 방문한 적 있는 노드가 무엇인지 저장할 배열 visited의 원소를 False로 초기화한다.
>3. 출발 노드를 방문처리한다.
>4. 출발 노드와 인접한 노드의 거리를 distance 배열에 대입한다.
>5. 출발 노드와 인접한 노드 중 가장 거리가 가까운 곳으로 이동한다.

>6. 그 노드를 방문 처리하고, 그 노드와 인접한 다른 노드들 사이의 '출발점부터 그 다른 노드까지의 거리'들을 계산하여 최단 거리 배열의 값을 갱신한다.
>7. 방문한 적 없고 최단 거리가 가장 작은 노드를 방문하여 6을 수행한다. 모든 노드들을 방문할 때까지 이를 반복한다.  

#### 코드
```python
from sys import stdin

INF = int(1e9)

input = stdin.readline

n, m = map(int, input().split())
st = int(input())
graph = [[] for i in range(n + 2)]
for i in range(m):
    u, v, w = map(int, input().split())
    graph[u].append((v, w))

visited = [False] * (n + 1)
distance = [INF] * (n + 1)


def get_smallest_node():
    min_value = INF
    index = 0
    for i in range(1, n + 1):
        if visited[i] == False and distance[i] < min_value:
            min_value = distance[i]
            index = i
    return index


def dijkstra(start):
    distance[start] = 0
    visited[start] = True
    for node in graph[start]:
        distance[node[0]] = node[1]

    for _ in range(2, n + 1):
        now = get_smallest_node()
        visited[now] = True
        for k in graph[now]:
            new_distance = distance[now] + k[1]
            if new_distance < distance[k[0]]:
                distance[k[0]] = new_distance


dijkstra(st)

print(distance[1:])

```

### 최소 힙을 사용하면 더 빠르게 동작한다.
```python
from sys import stdin
import heapq

INF = int(1e9)

input = stdin.readline

n, m = map(int, input().split())
st = int(input())
graph = [[] for i in range(n + 2)]
for i in range(m):
    u, v, w = map(int, input().split())  # u -> v 의 거리가 w.
    graph[u].append((w, v))  # 거리(w), 노드 번호(v).

distance = [INF] * (n + 1)


def dijkstra(start):
    h = []
    heapq.heappush(h, (0, st))
    distance[start] = 0
    while h:
        now = heapq.heappop(h)
        if distance[now[1]] < now[0]:
            continue
        for node in graph[now[1]]:
            new_distance = now[0] + node[0]
            if distance[node[1]] > new_distance:
                heapq.heappush(h, (new_distance, node[1]))
                distance[node[1]] = new_distance


dijkstra(st)

print(distance[1:])
```



## 플로이드 워셜 알고리즘
- 모든 정점에서 다른 모든 정점까지의 최단거리를 구하는 알고리즘이다.

### 알고리즘
>1. 인접한 노드 사이의 거리를 graph에 인접 행렬 방식으로 입력받는다.
>2. 시작점과 도착점이 같은 경로의 최단 거리는 0이므로, 시작점과 도착점이 같은 원소에 0을 대입한다.
>3. 거쳐갈 노드를 설정하고, 그 노드를 거치는 모든 시작점과 도착점 쌍에 대해 최단 경로를 계산한다.
>4. 3을 모든 노드에 대해 반복한다.

### 코드
```python 
INF = int(1e9)

n, m = map(int, input().split())
graph = [[INF] * (n + 1) for i in range(n + 1)]
for _ in range(m):
    a, b, c = map(int, input().split())
    graph[a][b] = c

for i in range(n + 1):
    for j in range(n + 1):
        if i == j:
            graph[i][j] = 0

for node in range(1, n + 1):
    for a in range(1, n + 1):
        for b in range(1, n + 1):
            new_distance = graph[a][node] + graph[node][b]
            graph[a][b] = min(graph[a][b], new_distance)

print(graph)
```
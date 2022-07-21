# DFS vs BFS

그래프 탐색 알고리즘

> **그래프**
>
> 정점과 간선으로 이루어진 자료구조

![dfs-bfs](https://imgur.com/LJsNUnm.png)

<br>

## DFS(Depth First Search)

**깊이 우선 탐색**이라는 의미로, 임의의 노드에 대해 해당 브랜치를 모두 탐색하고 다른 브랜치를 탐색하는 방법

Stack 자료구조 또는 재귀 함수를 사용하여 구현한다.

<br>

### 예시 코드 - Python

```python
dfs_result = []

is_visited = [False] * node_count
stack = deque()

stack.append(start_node)
while stack:
    popped = stack.pop()
    if is_visited[popped]:
        continue

    is_visited[popped] = True
    dfs_result.append(popped)
    for node in graph[popped]:
        if not is_visited[node]:
            stack.append(node)
```

<br>

## BFS(Breadth First Search)

**너비 우선 탐색**이라는 의미로, 임의의 노드로부터 인접한 노드들을 먼저 탐색하는 방법

Queue 자료구조를 사용하여 구현한다.

<br>

### 예시 코드 - Python

```python
bfs_result = []

is_visited = [False] * node_count
queue = deque()

queue.append(start_node)
is_visited[start_node] = True
while queue:
    popped = queue.popleft()
    bfs_result.append(popped)
    for node in graph[popped]:
        if not is_visited[node]:
            is_visited[node] = True
            queue.append(node)
```

<br>

## 문제 유형별 적합한 알고리즘 ⭐
- 최소 비용을 구하는 문제: BFS
    - DFS를 사용하면 처음 발견되는 값이 최소 비용이 아닐 수 있지만, **BFS를 사용하면 처음 발견되는 값이 최소 비용임을 보장한다.**
- 모든 정점을 방문해야 하는 문제: DFS, BFS
- 경로에 특정 속성을 기록해야 하는 문제: DFS
    - BFS는 경로의 속성을 기록할 수 없다.
- 그래프의 규모가 큰 문제: DFS
    - **DFS는 단일 경로를 기억하여 선형 메모리 공간을 사용**하고, BFS는 연결된 모든 방향으로 메모리 공간을 사용한다.
- 그래프의 규모가 크지 않고, 시작 지점으로부터 목적지가 멀지 않은 문제: BFS

<br>

## 📚 Reference

- https://velog.io/@lucky-korma/DFS-BFS%EC%9D%98-%EC%84%A4%EB%AA%85-%EC%B0%A8%EC%9D%B4%EC%A0%90

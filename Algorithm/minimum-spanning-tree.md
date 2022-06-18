# 최소 신장 트리(MST, Minimum Spanning Tree)

신장 트리 중에서 간선들의 가중치 합이 최소인 트리

> **신장 트리(Spanning Tree)**
>
> 그래프의 모든 정점을 방문하는 트리

<br>

## MST 특징

- 간선들의 가중치 합이 최소이다.
- 사이클이 존재하면 안된다.
- n-1개의 간선이 사용되야 한다.

<br>

## MST 구현 방법

- Kruskal 알고리즘
- Prim 알고리즘

<br>

## Kruskal 알고리즘

### 특징

- **그리디 방식**으로 간선을 선택하며 진행한다.

### 알고리즘

1. 그래프의 간선들을 가중치 기준으로 오름차순 정렬한다.
2. 우선순위 큐로 아래의 두 조건을 만족하는 간선들을 선택한다.
   1. 가중치가 낮은 간선 먼저
   2. 사이클이 형성되지 않도록

### 예시 코드 - Python

```python
# 1. & 2.1 가중치 기준 오름차순 우선순위 큐
heap = []
for edge in edges:
    heapq.heappush(heap, [edge.weight, edge.node1, edge.node2])

# 2-2. 사이클 확인을 위한 Disjoint-Set(Union-Find)
parents = [i for i in range(node_count + 1)]

def find_parent(a):
    if a == parents[a]:
        return a
    parents[a] = find_parent(parents[a])
    return parents[a]

def is_same_parents(a, b):
    return find_parent(a) == find_parent(b)

def union_parents(a, b):
    a_parent = find_parent(a)
    b_parent = find_parent(b)
    if a_parent < b_parent:
        parents[a_parent] = b
    else:
        parents[b_parent] = a

# 2. 두 조건을 모두 만족하는 간선 선택
min_weight = 0
while heap:
    weight, node1, node2 = heapq.heappop(heap)
    if not is_same_parents(node1, node2): # 사이클 확인
        union_parents(node1, node2) #
        min_weight += weight

print(min_weight)
```

<br>

## Prim 알고리즘

### 특징

- 시작 정점으로부터 확장해나간다.
- 정점 기반 알고리즘

### 알고리즘

1. 시작 정점을 신장 트리에 추가한다.
2. 신장 트리에 존재하지 않는 최저 가중치의 간선을 선택한다.
3. 신장 트리가 n-1개의 간선을 가질 때까지 반복한다.

### 예시 코드 - Python

```python
graph = defaultdict(list)
for edge in range(edges):
    graph[edge.node1].append([edge.weight, edge.node2])
    graph[edge.node2].append([edge.weight, edge.node1])

heap = []
visited_set = set()

# 1. 시작 정점 1번
visited_set.add(1)
for weight, other_node in graph[1]:
    heapq.heappush(heap, [weight, 1, other_node])

min_weigth = 0
while heap:
    weight, node1, node2 = heapq.heappop(heap)

    # 2. 신장 트리에 존재하는 정점인지 확인
    if set([node1, node2]).issubset(visited_set):
        continue

    new_node = node1 if node1 not in visited_set else node2

    min_weigth += weight
    visited_set.add(new_node)
    for other_weight, other_node in graph[new_node]:
        heapq.heappush(heap, [other_weight, new_node, other_node])

print(min_weight)
```

<br>

## 참고

- https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html
- https://gmlwjd9405.github.io/2018/08/30/algorithm-prim-mst.html

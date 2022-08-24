# Trie
트라이란 **문자열을 효율적으로 탐색하기 위한 트리 형태의 자료구조**이다.

자동 완성 기능, 사전 검색 등 문자열 탐색에 특화되었으며, 탐색 트리(re**trie**val tree)에서 나온 단어이다.

> **Tree**
> 
> 노드로 이루어진 계층 자료 구조로 나뭇가지처럼 연결된 비선형 자료구조이다.

![trie](https://imgur.com/AxxUo0K.png)

<br>

## Trie 구현 - Python

> 문제: [백준 - 14425번 문자열 집합](https://www.acmicpc.net/problem/14425)

### Node 설명
```python
class Node:
    def __init__(self, data):
        self.data = data # 노드에 담긴 문자를 나타내는 변수
        self.is_end = False # 해당 문자로 끝나는 단어가 존재함을 뜻하는 변수
        self.children = {} # 이어지는 노드들을 담는 배열
```


### 전체 코드

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.is_end = False
        self.children = {}


def insert_node(trie, word):
    node = trie
    for ch in word:
        if ch not in node.children:
            new_node = Node(ch)
            node.children[ch] = new_node
        node = node.children[ch]
    node.is_end = True


def is_included(trie, word):
    node = trie
    for ch in word:
        if ch not in node.children:
            return False
        node = node.children[ch]
    return node.is_end


N, M = map(int, sys.stdin.readline().split())
trie = Node(None)

for _ in range(N):
    word = sys.stdin.readline().rstrip()
    insert_node(trie, word)

count = 0
for _ in range(M):
    word = sys.stdin.readline().rstrip()
    if is_included(trie, word):
        count += 1
print(count)
```

<br>

## 📚 Reference
- https://velog.io/@kimdukbae/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%ED%8A%B8%EB%9D%BC%EC%9D%B4-Trie
- https://www.acmicpc.net/problem/14425
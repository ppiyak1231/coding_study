# 알고리즘 공부: 큐(Queue)

### 선입선출(FIFO) 구조: 처음 넣은 데이터를 처음 꺼내는 구조

- enqueue: 데이터를 추가하는 작업
- dequeue: 데이터를 꺼내는 작업
- front: 큐의 맨 앞 데이터를 반환(제거하지는 않음)
- rear: 큐의 맨 뒤 데이터를 반환(제거하지는 않음)
- is empty: 큐가 비어 있는지 여부 확인
- size: 큐에 저장된 데이터 개수를 반환

---

### 이때!

파이썬에서 배열(List)로 구현한 큐는 굉장히 비효율적

(디큐 작업에서 리스트 앞쪽을 빼면 내부의 전체 index가 하나씩 당겨지면서, 전체 메모리 위치가 변경되기 때문)

insert와 pop 사용을 지양하고 deque(덱)를 사용

```python
from collections imort deque

queue = deque([1, 2, 3])
first_element = queue.popleft()
```

---

### 큐 활용 사례

- 작업 스케줄링: ready 큐에 순서대로 들어온 프로세스를 순차적으로 실행하기
- 네트워크 패킷 처리: 데이터 전송 시 큰 데이터를 작은 조각으로 분할하여 패킷으로 만들고 이를 순서대로 저장→처리하여 패킷 손실 방지하고 네트워크 효율 향상
- 멀티스레딩: 자원에 대한 요청을 큐에 저장하고 순서대로 자원을 할당
- 너비 우선 탐색(BFS): 탐색할 노드를 큐에 저장하고 순차적으로 방문하여 그래프 탐색

---

```python
class Queue:
    def __init__(self):
        self.items = []

    def isEmpty(self):
        if self.items == []:
            return True
        else:
            return False

    def enqueue(self, item):
        self.items.append(item)

    def dequeue(self):
        if len(self.items) != 0:
            return self.items.pop(0)
        else:
            print("Queue is empty")

    def display(self):
        return (self.items)
```

### 관련 문제(서버 증설 횟수): https://school.programmers.co.kr/learn/courses/30/lessons/389479


### 풀이

```python
from collections import deque

def solution(players, m, k):
    # 서버를 위한 큐
    server_queue = deque([0]*k)
    server_count = 0
    
    for player in players:
        # 만료 서버 제거
        server_queue.popleft()
        
        current_server = sum(server_queue)
        
        required_server = player // m
        
        # 필요한 서버가 현재 서버보다 많으면 증설
        if required_server > current_server:
            added_server = required_server - current_server
            
            server_count += added_server
            server_queue.append(added_server)
        
        # 아니면 0 넣어 큐 길이는 유지
        else:
            server_queue.append(0)
    
    answer = server_count
    return answer
```


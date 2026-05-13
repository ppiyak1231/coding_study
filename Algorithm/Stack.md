# 알고리즘 공부: 스택(Stack)
### 후입선출(LIFO, Last-In-First-Out) 구조: 가장 나중에 넣은 데이터를 가장 먼저 꺼내는 구조

- push: 데이터를 스택의 맨 위에 추가하는 작업
- pop: 스택의 맨 위에서 데이터를 꺼내는(제거하는) 작업
- top (또는 peek): 스택의 맨 위 데이터를 반환 (제거하지는 않음)
- is_empty: 스택이 비어 있는지 여부 확인
- size: 스택에 저장된 데이터 개수를 반환

---

### 파이썬에서의 스택 구현
파이썬에서 리스트(List)로 구현한 스택은 큐(Queue)와 달리 효율에서 문제 없음.
데이터를 맨 뒤에 추가하는 append()와 맨 뒤에서 빼내는 pop() 연산은 내부 데이터의 위치 이동이 발생하지 않아 시간 복잡도가 O(1) 이기 때문.

```
stack = [1, 2, 3]
stack.append(4)            # 스택에 데이터 추가 (push)
top_element = stack.pop()  # 스택의 맨 위 데이터 꺼내기 (pop)
```

### 스택 활용 사례
- 함수 호출 (Call Stack): 프로그램에서 함수가 호출될 때 메모리에 되돌아갈 주소와 지역 변수를 저장하고, 실행이 끝나면 이전 상태로 복귀할 때 사용
- 웹 브라우저 방문 기록 (뒤로 가기): 사용자가 방문한 페이지를 순서대로 쌓아두고, '뒤로 가기' 버튼을 누르면 가장 최근(맨 위) 페이지부터 꺼내어 이동
- 실행 취소 (Undo): 워드프로세서나 이미지 편집기에서 사용자의 동작을 순서대로 기록하고, 'Ctrl+Z'를 누르면 직전 작업을 취소
- 괄호 검사: 수식이나 소스 코드에서 여는 괄호와 닫는 괄호의 짝과 순서가 올바르게 맞는지 확인할 때 사용
- 깊이 우선 탐색 (DFS): 그래프나 트리 탐색 시 스택(또는 재귀 함수)을 사용하여 더 이상 갈 곳이 없을 때까지 깊숙하게 노드를 방문

---

```python
class Stack:
    def __init__(self):
        self.items = []

    def is_empty(self):
        if self.items == []:
            return True
        else:
            return False

    def push(self, item):
        self.items.append(item)

    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        else:
            print("Stack is empty")
            
    def top(self):
        if not self.is_empty():
            return self.items[-1] # 리스트의 맨 마지막 요소 반환
        else:
            print("Stack is empty")

    def display(self):
        return self.items

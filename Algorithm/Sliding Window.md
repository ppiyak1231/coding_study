# 알고리즘 공부: 투 포인터 & 슬라이딩 윈도우

### 연속된 구간을 효율적으로 탐색하는 알고리즘

- **투 포인터(Two Pointers):** 두 개의 포인터(인덱스)를 조작하며 범위를 조절하는 방식 (구간의 크기가 가변적일 때 주로 사용)
- **슬라이딩 윈도우(Sliding Window):** 고정된 크기 혹은 가변적인 크기의 창문(Window)을 옆으로 밀어가며 탐색하는 방식
- **시작 날짜 / left / start:** 탐색 구간의 왼쪽 끝 (주로 조건 만족 시 구간을 줄이는 역할)
- **종료 날짜 / right / end:** 탐색 구간의 오른쪽 끝 (주로 구간을 확장하며 새로운 데이터를 수집하는 역할)

---

### 이때

모든 구간의 조합을 일일이 다 검사하는 방식(브루트 포스)을 사용하면 시간 복잡도가 $O(N^2)$이 되어 데이터가 많을 때 시간 초과가 발생.

하지만 투 포인터를 사용하면 `start`와 `end` 포인터가 각각 처음부터 끝까지 **오직 앞으로만** 이동하므로, 단 **$O(N)$** 만에 문제를 효율적으로 해결.

데이터의 중복을 처리하고 앞부분을 잘라낼 때 빈도수를 안전하게 체크하기 위해 주로 **해시(파이썬의 딕셔너리)** 자료구조와 함께 조합되어 사용.

```python
# 파이썬 딕셔너리 안전하게 사용하기 (에러 방지)
# 장부에 이름이 없으면 0을 가져오고, 있으면 기존 개수를 가져와서 +1
actor_count[current_actor] = actor_count.get(current_actor, 0) + 1

# 장부에서 데이터 안전하게 삭제하기
actor_count.pop(left_actor)
```

---

### 투 포인터 / 슬라이딩 윈도우 활용 사례

- **구간 합 구하기:** 배열에서 연속된 부분 배열의 합이 특정 target이 되는 최단/최장 구간 찾기
- **중복 문자 없는 가장 긴 부분 문자열:** 문자열을 탐색하며 문자 중복이 발생하지 않는 최소 범위 구하기
- **모든 종류의 아이템 수집:** 주어진 목록에서 지정된 모든 종류의 아이템을 최소 몇 개만에 다 모을 수 있는지 계산 (쇼핑, 관람 등)

---

### 기본 코드 구조 (가변 크기 슬라이딩 윈도우)

```python
def sliding_window_template(arr, K):
    count_map = {}
    start = 0
    min_length = float("inf")

    for end in range(len(arr)):
        # 1. 오른쪽 포인터(end) 전진하며 데이터 수집 (머리 늘리기)
        current = arr[end]
        count_map[current] = count_map.get(current, 0) + 1

        # 2. 특정 조건(예: 종류 수가 K개)을 만족하는 동안
        while len(count_map) == K:
            # 3. 최솟값 또는 정답 갱신
            min_length = min(min_length, end - start + 1)

            # 4. 왼쪽 포인터(start) 당기며 다이어트 (꼬리 줄이기)
            left_item = arr[start]
            count_map[left_item] -= 1
            
            # 카운트가 0이 되면 장부에서 완전히 제거
            if count_map[left_item] == 0:
                count_map.pop(left_item)
                
            start += 1
            
    return min_length
```

### 관련 문제(뮤지컬): 최소 일수 구하기

### 문제

문제설명
철수는 뮤지컬 마니아입니다. 철수는 뮤지컬 A가 이웃 도시에서 공연된다는 소식을 들었습니다. A는 N일 동안 하루에 한 번씩 공연됩니다.

A의 어떤 배역은 K명의 배우가 날마다 번갈아가며 연기합니다. 각 배우들에는 1번부터 K번까지 번호가 붙어있습니다. 철수는 해당 배역을 매우 좋아하기 때문에 배우 K명의 연기를 모두 보고싶습니다.

철수는 이웃 도시의 호텔에 머물면서 A를 관람하려고 합니다. 철수는 이웃 도시에 머무는 동안 매일 빠짐없이 A를 관람합니다.

철수가 이웃 도시의 호텔에 머물러야 하는 최소 일수를 구하는 프로그램을 작성해주세요.

예를 들어 N = 7, K = 3, 날마다 연기하는 배우들의 번호가 2, 1, 2, 2, 3, 2, 3이라고 가정하겠습니다. 철수가 두 번째 날부터 다섯 번째 날까지 총 4일 동안 이웃 도시의 호텔에 머물면서 뮤지컬을 관람하면 모든 배우들의 연기를 볼 수 있습니다.

입출력 예

```
입력 #1

7 3

2 1 2 2 3 2 3

---

입력 #2

5 5

1 2 3 4 5
```

입력값 설명

첫째 줄에 뮤지컬이 공연되는 일수 N과 같은 배역을 맡은 배우들의 수 K가 공백으로 구분되어 주어집니다. (2 ≤ K ≤ N ≤ 10,000)
둘째 줄에 i번째 날에 연기하는 배우의 번호 a_i가 공백으로 구분되어 주어집니다. (1 ≤ a_i ≤ K)
모든 배우가 최소 한 번 이상 연기하는 입력만 주어집니다.

```
출력 #1

4

---

출력 #2

5
```

출력값 설명

첫째 줄에 철수가 이웃 도시의 호텔에 머물러야 하는 최소 일수를 출력합니다.

---

### 풀이

```python
import sys 

N, K = map(int, sys.stdin.readline().split()) 
actors = list(map(int, sys.stdin.readline().split()))

actor_count = {} # 현재 구간에 포함된 배우들 누구인지 + 몇 번 봤는지
start_day = 0 # 시작 날짜, end_day: 종료 날짜
min_days = 10000 # 최대 1만 day

for end_day in range(N):
    current_actor = actors[end_day] # 오늘의 배우
    
    if current_actor in actor_count: # 배우 기록하기
        actor_count[current_actor] += 1  # 이미 있다면 개수 +1
    else:
        actor_count[current_actor] = 1  # 처음 등장했다면 1로 등록
    
    while len(actor_count) == K: # 만약 배우가 모두 있다면(총 K명)
        # 머문 일 수 계산
        min_days = min(min_days, end_day - start_day + 1)
        
        # 시작 날짜의 배우가 누구인가(구간 줄일 거라서)
        start_actor = actors[start_day]
        
        # 해당 배우 카운트 차감(뺐으니)
        actor_count[start_actor] -= 1
        
        # 만약 배우를 본 횟수가 0이 되면 해당 기록은 제거(while 문에 의해) 
        if actor_count[start_actor] == 0:
            actor_count.pop(start_actor)
        
        # 구간 줄이기
        start_day += 1
        
print(min_days)
```
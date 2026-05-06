# DP - 누적합(Prefix Sum)

### 이론

- 동적 계획법(DP)의 형태 중 하나로 누적합을 계산하여 다른 배열에 저장했다가 특정 구간의 합을 구할 때 저장해둔 누적합 배열을 사용
- 조건: 누적합 배열의 값들은 변하지 않아야 한다.

---

### 1차원 누적합

<img width="1280" height="379" alt="img1 daumcdn" src="https://github.com/user-attachments/assets/319ad430-5e0f-4891-a31a-4ff8c0e20304" />

```python
prefix_sum[i] = prefix_sum[i-1] + array[i]

----

def get_prefix_sum(data):
    # 1. 누적합 배열 초기화 (0번째 인덱스는 0으로 설정)
    prefix_sum = [0] * (len(data) + 1)
    
    # 2. 누적합 계산
    for i in range(len(data)):
        prefix_sum[i + 1] = prefix_sum[i] + data[i]
    
    return prefix_sum
```

### 1차원 구간합

<img width="1280" height="217" alt="img1 daumcdn" src="https://github.com/user-attachments/assets/0e33d73f-e019-488d-add2-2de45742f302" />

```python
range(x, y) = prefix_sum[y] - prefix_sum[x-1]

----

result = p_sum[right + 1] - p_sum[left]
```

---

### 2차원 누적합

<img width="501" height="475" alt="img1 daumcdn" src="https://github.com/user-attachments/assets/376a929c-7625-4796-8d0f-abc96ef892b5" />

<img width="719" height="390" alt="img1 daumcdn" src="https://github.com/user-attachments/assets/ccf3e89f-e79e-4f0e-b0ca-3e64589e0240" />

```python
prefix_sum[x, y] = arr[x-1, y-1] + prefix_sum[x-1, y] + prefix_sum[x, y-1] - prefix_sum[x-1, y-1]

----

def get_2d_prefix_sum(matrix):
    rows = len(matrix)
    cols = len(matrix[0])
    # 패딩을 위해 (rows+1) x (cols+1) 크기로 생성
    S = [[0] * (cols + 1) for _ in range(rows + 1)]
    
    for i in range(1, rows + 1):
        for j in range(1, cols + 1):
            S[i][j] = matrix[i-1][j-1] + S[i-1][j] + S[i][j-1] - S[i-1][j-1]
    return S
```

### 2차원 구간합

<img width="501" height="475" alt="img1 daumcdn" src="https://github.com/user-attachments/assets/403a1e90-93d7-4bc7-ac28-45d7c407ea27" />

```python
range(x1,y1 ~ x1, y2) = prefix_sum[x2, y2] - prefix_sum[x1 -1, y2] - prefix_sum[x2, y1 -1] + prefix_sum[x1 -1, y1 -1]

----

result = S[x2+1][y2+1] - S[x1][y2+1] - S[x2+1][y1] + S[x1][y1]
```


---

### 파이썬에서는 라이브러리로 쉽게 구현

```python
from itertools import accumulate

data = [10, 20, 30, 40, 50]
# accumulate는 이터레이터를 반환하므로 리스트로 변환
prefix_sum = [0] + list(accumulate(data))

print(prefix_sum) # [0, 10, 30, 60, 100, 150]
```

이미지 출처: https://code-angie.tistory.com/22

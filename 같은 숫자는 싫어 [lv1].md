# 같은 숫자는 싫어 [lv1]

### 🌐 문제 링크:

https://school.programmers.co.kr/learn/courses/30/lessons/12906?language=python3

# 💻 문제 설명

- 
    
    숫자 0부터 9까지 이루어져 있는 배열 arr을 주어졌을 때, 배열에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 한다. 단, 제거된 후 남은 수들을 반환할 때에는 배열 arr의 원소들의 순서를 유지해야 한다.
    

# **💡 풀이 과정**

## 문제 접근

주어진 배열에서 중복된 숫자를 제거하기 위해 배열을 순회하면서 현재 값과 이전 값을 비교합니다.

- **현재 값과 이전 값이 다를 경우**: 결과 배열에 추가합니다.
- **현재 값과 이전 값이 같을 경우**: 해당 반복을 건너뛰어 다음 값을 비교합니다.

단, 첫 번째 값은 비교 대상이 없으므로 초기화 과정에서 새 배열에 미리 추가하고, 반복문은 두 번째 값부터 시작하도록 설정합니다. 이를 통해 중복을 제거한 배열을 얻을 수 있습니다.

# ✏️ **풀이 코드**

```python
def solution(arr):
    ans = []
    ans.append(arr[0])
    for i in range(1, len(arr)):
        if ans[-1] != arr[i]:
            ans.append(arr[i])
    return ans
```

# ✏️ **풀이 코드(stack 사용)**

```python
from collections import deque

def solution(arr):
    stack = deque()  
    for num in arr:
        if not stack or stack[-1] != num: 
            stack.append(num)
    return list(stack)
```

현재 문제풀이는 자료의 맨 끝에서 작업을 하기 때문에,  리스트와 스택(deque)은 모두 O(1)의 시간 복잡도를 가진다. 따라서, 맨 끝에서만 요소를 추가/삭제하는 작업은 리스트와 스택의 성능이 동일하다.

# ✏️ **다른 사람 풀이 코드**

```python
def solution(arr):
    ans= []
    for i in arr:
        if ans[-1:] != [i]:
            ans.append(i)
    return ans
```

[-1:]와 같은 리스트 슬라이싱을 사용하는 경우, 리스트의 마지막 요소를 가져오며, 리스트가 비어 있어도 오류 없이 빈 리스트([])를 반환하므로 안전하게 비교할 수 있습니다.

또한, 슬라이싱은 원본 리스트를 복사하여 사용하기 때문에, 슬라이싱 결과를 수정하더라도 원본 리스트에 영향을 주지 않습니다.

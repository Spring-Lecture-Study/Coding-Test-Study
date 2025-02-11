# 이중 우선순위 큐 [lv3]

### 🌐 문제 링크:

https://school.programmers.co.kr/learn/courses/30/lessons/42628

# 💻 문제 설명

- 
    
    이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.
    
    | 명령어 | 수신 탑(높이) |
    | --- | --- |
    | I 숫자 | 큐에 주어진 숫자를 삽입합니다. |
    | D 1 | 큐에서 최댓값을 삭제합니다. |
    | D -1 | 큐에서 최솟값을 삭제합니다. |
    
    이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.
    

# **💡 풀이 과정**

## 문제 접근

문제에서 명령어에 따라 큐에 있는 최대값과 최소값을 삭제해야 한다. 

최대값 최소값 접근하는 방법은 여러 가지가 있으나, 제일 효율적인 방법은 **heap** 자료구조를 사용하는 것이다. → 삽입/삭제 연산이 **O(log N)**의 시간 복잡도를 가지므로 효율적이다.

그런데 처음에는 최소힙(min-heap)만 사용하면 내장 함수로 최소값을 쉽게 가져올 수 있고, 최대값은 리스트의 [-1]을 통해 접근할 수 있을 것이라고 생각했다. 

하지만 힙은 **완전 이진 트리 구조**이기 때문에, 리**스트의 마지막 요소가 항상 최댓값이 아닐 수도 있다.**

ex/ 최소힙

```
         3
       /   \
      5     7
     / \   / \
   10  15 20 17
```

- 최소힙에서 부모 노드는 자식 노드보다 작기만 하면 되고, 자식 노드들끼리는 크기 순서가 보장되지 않는다. → [3, 5, 7, 10, 15, 20, 17]

이 문제를 해결하려면 최소힙과 최대힙을 따로 구성하여 **두 개의 힙**을 사용해야 한다.

각 명령어에 따라 

최대값을 삭제해야 한다면 **최대힙**을 사용하고,

최소값을 삭제해야 한다면 **최소힙**을 사용한다.

단, 이러한 값들을 가져와 삭제한다면 다른 힙에도 동기화하여 두 힙이 서로 일치하게 해야한다.

## 코드 접근

### 명령어 분리

```python
for cmd in operations:
	lst = cmd.split()
```

각 명령어는 **문자열**로 되어 있으며, **공백**을 기준으로 연산 종류(I, D)와 숫자 값을 구분할 수 있습니다. 따라서, split() 함수를 사용하면 명령어를 두 부분으로 나눠 필요한 값을 쉽게 접근할 수 있습니다.

### 최대값 최소값 삭제

```python
if lst[1] == "1":
  max_num = -heapq.heappop(max_heap)
  min_heap.remove(max_num)
elif lst[1] == "-1":
  min_num = heapq.heappop(min_heap)
  max_heap.remove(-min_num)
```

최소힙에서는 루트 노드가 최소값이기 때문에, 최대값을 쉽게 얻으려면 최소힙의 특성을 반전시켜야 합니다.

**최대값을 처리할 때**
최대힙은 최소힙으로 구현되었지만, 값을 **음수**로 저장함으로써 최대값을 루트 노드에 배치할 수 있게 만듭니다.
초대힙에서 최소값을 꺼낼 때, 음수로 저장한 값이 최대값이므로 이를 다시 **부호를 반전**시켜 최대값을 반환합니다.
**최소값을 처리할 때**
힙은 기본적으로 최소힙이므로, heapq.heappop()에서 꺼내면 자연스럽게 최소값이 나옵니다.
최대값에 동기화할 때에는 값을 음수로 변환하여, 최대값에서 해당 값을 제거합니다.

- 이는 max_heap이 음수 값들로 구성되어 있기 때문입니다.

따라서, 최대값을 추출하려면 음수로 저장하고, 최소값은 그대로 저장한 상태에서 힙을 다루면 됩니다.

# ✏️ **풀이 코드**

```python
import heapq

def solution(operations):
    min_heap = []
    max_heap = []

    for command in operations:
        cmd = command.split()
        
        if cmd[0] == "I":
            num = int(cmd[1])
            heapq.heappush(min_heap, num)
            heapq.heappush(max_heap, -num)
            
        elif cmd[0] == "D" and min_heap:
            if cmd[1] == "1":
                max_num = -heapq.heappop(max_heap)
                min_heap.remove(max_num)
            elif cmd[1] == "-1":
	                min_num = heapq.heappop(min_heap)`
                max_heap.remove(-min_num)
                
    if min_heap:
        return [-heapq.heappop(max_heap), heapq.heappop(min_heap)]  
    else:
        return [0, 0]
```

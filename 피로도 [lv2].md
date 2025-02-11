# 피로도 [lv2]

### 🌐 문제 링크:

https://school.programmers.co.kr/learn/courses/30/lessons/87946

# 💻 문제 설명

- 
    
    XX게임에는 피로도 시스템이 있으며, 일정 피로도를 사용해서 던전을 탐험할 수 있습니다. 던전마다 탐험을 시작하기 위해 필요한 "최소 필요 피로도"와 던전 탐험을 마쳤을 때 소모되는 "소모 피로도"가 있습니다.
    
    유저의 현재 피로도 k와 각 던전별 "최소 필요 피로도", "소모 피로도"가 담긴 2차원 배열 dungeons 가 매개변수로 주어질 때, 유저가 탐험할수 있는 최대 던전 수를 return 하도록 solution 함수를 완성해주세요.
    

# **💡 풀이 과정**

## 문제 접근

문제는 유저의 피로도를 고려해서 하루의 유저가 탐험할 수 있는 최대 던전의 수를 구하는 문제입니다. 

조건에 맞게 모든 경우의 수를 구하면서 최대값을 구하는 문제이기 때문에 **DFS(깊이 우선 탐색)와 백트래킹**을 활용해야 한다고 생각했습니다.

**DFS 탐색 방식**

반복문을 통해 모든 던전을 순회하면서 **현재 피로도가 해당 던전의 최소 필요도 이상이고**, **해당 던전을 이전에 방문하지 않았을 때** 탐험 하고 방문 표시를 해준다.
그렇게 소모 피로도를 뺀 값과 탐헛 횟수를 증가시킨 후 다음 DFS를 호출한다.
다른 경우의 수를 탐색할 수 있도록 방문 표시를 원래 상태로 복구하고(백트래킹), 모든 던전을 확인한 후에도 갈 곳이 없다면 dfs를 종료시킨다.

**최대 탐험 횟수 갱신 방식**
DFS는 호출될 때마다 현재까지 탐험한 던전의 개수를 max_ans와 비교하여 갱신한다.

- 이는 각 호출 시점에서의 탐험 횟수가 해당 경로에서 도달할 수 있는 최댓값이기 때문이다.

## 코드 접근

```python
dfs(hp, ans, dungeons, visited):
```

visited같은 경우 global 키워드를 사용해서 전역변수로 초기화 하고 dfs 함수내에서 접근하는 방법이 있지만, dungeons같은 경우 전역변수로 선언해도 solution 함수에 매개변수 이름과 같기 때문에 이를 방지하기 위해 dfs 호출시 매개변수로 넘겨준다.

- 전역 변수를 사용하는 것보다 매개변수로 넘겨주는 것이 코드의 **가독성**, **디버깅 용이성**, **재사용성**, **상태 관리** 등 여러 측면에서 더 좋다.

# ✏️ **풀이 코드**

```python
def dfs(hp, ans, dungeons, visited):
    global max_ans
    max_ans = max(max_ans, ans)  

    for i in range(len(dungeons)):  
        if not visited[i] and hp >= dungeons[i][0]:  
            visited[i] = True  
            dfs(hp - dungeons[i][1], ans + 1, dungeons, visited)
            visited[i] = False  

def solution(k, dungeons):
    global max_ans
    max_ans = 0
    visited = [False] * len(dungeons) 
    dfs(k, 0, dungeons, visited)  
    return max_ans
```

# ✏️ **풀이 코드 2**

```python
def solution(k, dungeons):
    max_count = 0
    
    def dfs(current_k, count):
        nonlocal max_count
        max_count = max(max_count, count)
        
        for i in range(len(dungeons)):
            if not visited[i] and current_k >= dungeons[i][0]:
                visited[i] = True
                dfs(current_k - dungeons[i][1], count + 1)
                visited[i] = False
                
    visited = [False] * len(dungeons)
    dfs(k, 0)
    
    return max_count
```

위 코드와 달리 해당 코드는 dfs 함수가 solution 함수 내에 정의되어 있기 때문에, solution 함수 내에서 선언된 변수들(dungeons)은 dfs 함수에서도 접근할 수 있습니다. 또한, dfs 기준으로 상위 함수인 solution에 max_count 변수를 수정하기 위해 **nonlocal**을 사용한다. 

- **nonlocal**은 해당 변수가 현재 함수에서 상위 함수(solution)에 있는 변수를 수정하도록 지정하는 키워드입니다.

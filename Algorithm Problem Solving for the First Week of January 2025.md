# 15649. N과 M (1)[S3]

### 🌐 문제 링크:

https://www.acmicpc.net/problem/15649

# 💻 문제 설명

- 
    
    자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.
    
    • 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
    
    **제한 사항**
    
    한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 **공백**으로 구분해서 출력해야 한다.
    
    수열은 사전 순으로 증가하는 순서로 출력해야 한다.
    

# **💡 풀이 과정**

## 문제 접근

문제는 1부터 N까지의 자연수 중에서 중복 없이 사전 순으로 증가하는 순서로 출력해야 한다.

이렇게 모든 가능한 경우를 탐색하면서 조건에 맞는 결과만 출력하는 **백트래킹 알고리즘**을 사용해야 한다.

**백트래킹 알고리즘**에 대해 먼저 간단하게 설명해 보면,

해결책을 찾기 위해 모든 가능한 경우의 수를 탐색하지만, 그 과정에서 불필요한 탐색을 줄이기 위해 조건에 맞지 않는 경우에는 탐색을 중단하고 이전 상태로 돌아가 다른 가능성을 시도하는 알고리즘이다.

간혹가다 브루트포스와 백트래킹, 그리고 DFS(깊이우선탐색)를 혼동하는 경우가 있어 이에 대해 정리를 하면,

### **브루트포스**

 말 그대로 **모든 경우의 수를 찾아보는 것**이다.

모든 경우의 수를 탐색하다보니 만족하는 값을 100% 찾아낸다는 것이 강점이다. 반대로 단점이라면 모든 경우의 수를 판단하는 만큼 조합 가능한 경우의 수가 많으면 많을 수록 자원을 매우 많이 필요로 한다는 점이다.

### **백트래킹**

**탐색 도중 조건에 맞지 않는 경로는 더 이상 진행하지 않고 되돌아가면서 다른 경로를 탐색하는 방식**으로, 불필요한 탐색을 줄여 효율성을 높인다.

### DFS(깊이우선탐색)

 트리순회의 한 형태다. 하나의 순회 알고리즘으로 **백트래킹에 사용하는 대표적인 탐색 알고리즘**이다. **즉, 백트래킹 = DFS가 아니라 백트래킹의 방법 중 하나가 DFS인 것이다.** 그 외에도 BFS(너비우선탐색) 등 다양한 방법으로 백트래킹을 구현할 수 있다.

https://st-lab.tistory.com/114

### **깊이 우선 탐색이란?**

탐색을 할 때 가능한 한 깊게, 즉 자식 노드를 먼저 방문한 후 그 자식 노드들에 대해 다시 탐색을 진행하는 방식을 의미한다.  이때, **백트래킹**을 이용하여 조건에 맞지 않는 경로는 더 이상 탐색하지 않고 이전 상태로 되돌아가며 다른 경로를 탐색한다.

문제에 적용하면, 수열을 노드로 생각하고, 깊이가 깊은 노드인 마지막 수열 값을 우선적으로 탐색하고, 조건에 부합하지 않으면 백트래킹을 통해 첫 번째 수열 값으로 돌아와서 탐색을 진행하게 된다.

수열을 저장하는 **arr 배열**과 각 번호가 탐색되었는지를 나타내는 boolean형 배열 **visit**을 만들어, depth가 M에 도달하면 해당 수열을 StringBuilder에 저장하고, depth가 M보다 작으면 1부터 N까지의 수를 탐색하면서 아직 방문하지 않은 수들만 선택해 수열을 만들어 계속 탐색한다.

# ✏️ **풀이 코드**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static int N;
    public static int M;
    public static int[] arr;
    public static boolean[] visit;
    public static StringBuilder sb = new StringBuilder();

    public static void dfs(int depth) {
        if (depth == M) {
            for (int val : arr) {
                sb.append(val).append(' ');
            }
            sb.append('\n');
            return;
        }

        for (int i = 0; i < N; i++) {
            if (!visit[i]) {
                visit[i] = true;
                arr[depth] = i + 1;
                dfs(depth + 1);
                visit[i] = false;
            }
        }
    }

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        arr = new int[M];
        visit = new boolean[N];

        dfs(0);
        System.out.println(sb);
    }
}
```

## 📋 주요 코드 설명

```java
public static void dfs(int depth) {
    if (depth == M) {
        for (int val : arr) {
            sb.append(val).append(' ');
        }
        sb.append('\n');
        return;
    }

    for (int i = 0; i < N; i++) {
        if (!visit[i]) {
            visit[i] = true;
            arr[depth] = i + 1;
            dfs(depth + 1);
            visit[i] = false;
        }
    }
```

DFS는 일반적으로 깊이에 만족하면 결과를 출력하고, 깊이가 맞지 않으면 반복문을 통해 값을 증가시키며 재귀적으로 호출한다.

이 코드에서는 깊이가 M에 도달하면, 수열이 저장된 arr을 StringBuilder에 공백을 구분자로 저장하여 출력하고, 이후 함수는 종료되어 이전 호출문으로 돌아간다. 이때 반복문의 현재 진행 상태를 유지하게 되면서, ****visit[i] = false 구문을 사용하여 다른 깊이에서 해당 숫자를 다시 사용할 수 있게 하여, 모든 가능한 수열을 순차적으로 탐색할 수 있게 만든다.

- 수열 [1,2]로 예를 들면,  visit[1]이 false로 바뀌어, 이후 다시 dfs(0)에서 숫자 2를 선택할 수 있게 된다. 이후 반복문이 진행되어 i 값이 2가 되면, visit[2]는 true로 설정되고, arr[1]에 2 + 1 값인 3이 저장된 뒤 dfs(2)가 호출된다.

# ✏️ **풀이 코드(Python)**

```python
import sys
input = sys.stdin.readline

def dfs(depth):
    if depth == M:
        print(" ".join(map(str, arr)))
        return

    for i in range(N):
        if not visit[i]:
            visit[i] = True
            arr[depth] = i + 1
            dfs(depth + 1)
            visit[i] = False

N, M = map(int, input().split())
arr = [0] * M
visit = [False] * N

dfs(0)
```

Python에서는 배열에 저장된 수열을 **' '.join(map(str, arr))**를 사용하여 공백을 기준으로 하나의 문자열로 변환할 수 있다. 이를 통해 수열을 문자열로 만들어 바로 출력한 뒤, 재귀 호출을 종료(return)한다.

# 15650. N과 M (2)[S3]

### 🌐 문제 링크:

https://www.acmicpc.net/problem/15650

# 💻 문제 설명

- 
    
    자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.
    
    - 1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
    - 고른 수열은 **오름차순**이어야 한다.
    
    **제한 사항**
    
    한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.
    
    수열은 사전 순으로 증가하는 순서로 출력해야 한다.
    

# **💡 풀이 과정**

## 문제 접근

저번 백트래킹 N과 M (1)와 같이 백트래킹으로 풀어야 하는 문제지만, 다른 점은 고른 수열을 오름차순이어야만 한다.

처음에는 문제에 대해 이해하지 못했지만, 예제 입력 3을 보니 이해하게 되었다.

이번 문제는 전 문제처럼 1부터 N까지의 자연수에서 M개의 수열을 구하는 문제지만 오름차순인 수열만 뽑아야 하기 때문에 해당 조건에 맞는 수열만 뽑는 방법을 생각해야 했다.

그래서 탐색 시 이전에 탐색한 숫자에 +1을 증가시켜, 이전 숫자 이후에 숫자만을 선택하도록 구현하여 오름차순 수열을 생성한다. 이때 dfs호출 할때 해당 변수를 같이 받도록 하여 반복문의 시작 값을 해당 변수값으로 시작하도록 하고, 재귀적으로 호출할 때는 해당 값을 깊이와 같이 증가시켜 호출하도록 한다.

# ✏️ **풀이 코드**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static int N;
    public static int M;
    public static int[] arr;
    public static StringBuilder sb = new StringBuilder();

    public static void dfs(int at, int depth) {

        if (depth == M) {
            for (int val : arr) {
                sb.append(val).append(' ');
            }
            sb.append('\n');
            return;
        }

        for (int i = at; i <= N; i++) {
            arr[depth] = i;
            dfs(i + 1, depth + 1);
        }
    }

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        arr = new int[M];

        dfs(1, 0);
        System.out.println(sb);

    }
}
```

## 📋 주요 코드 설명

```java
public static void dfs(int at, int depth) {

      if (depth == M) {
          for (int val : arr) {
              sb.append(val).append(' ');
          }
          sb.append('\n');
          return;
      }

      for (int i = at; i <= N; i++) {
          arr[depth] = i;
          dfs(i + 1, depth + 1);
      }
  }
```

dfs메서드 매개변수에 현재 위치, 즉 어디서 시작하는지를 의미하는 변수를 같이 받도록 하여, 재귀 호출할 때 깊이와 함께 +1 증가시킴으로써 다음 수열 값은 이전 값 이후의 값만 선택되도록 만든다.  

해당 코드는 visit 배열이 없는데 이유는  시작 위치를 매개변수로 전달함으로써, 탐색 범위를 자연스럽게 제한하고 중복 사용을 방지해주기 때문에 사용하지 않아도 된다.

# ✏️ **풀이 코드(Python)**

```python
import sys
input = sys.stdin.readline

def dfs(at, depth):
    if depth == M:
        print(" ".join(map(str, arr)))
        return

    for i in range(at, N + 1):
        arr[depth] = i
        dfs(i + 1, depth + 1)

N, M = map(int, input().split())
arr = [0] * M

dfs(1, 0)
```

dfs 재귀 호출할 때 (at + 1) 을 하게 되면 오답이 되는 이유는,

at + 1을 사용하면 깊이에 의한 탐색이기 때문에 이전 값들과 관계없이 수열을 탐색하므로, 오름차순이 보장되지 않게 된다. 따라서, 오름차순 수열을 만들기 위해서는 i + 1을 사용해야 한다.

예를 들어 입력값이 4 2 라면,

[1 4]까지 출력하고 return 하여 돌아갔을 때, dfs(1,0)호출했을 때로 돌아오게 되고 이때 for문을 1부터 시작했기 때문에 2로 넘겨져서 첫 번째 수열에 1이 들어가게 되고 재귀 호출을 하면서 dfs(2,1)이 되면서 반복문이 2부터시작하기 때문에 [2 2]가 출력되게 되면서 오름차순이 아니므로 오답 풀이가 된다.

# 15651. N과 M (3)[S3]

### 🌐 문제 링크:

https://www.acmicpc.net/problem/15651

# 💻 문제 설명

- 
    
    자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.
    
    - 1부터 N까지 자연수 중에서 M개를 고른 수열
    - 같은 수를 여러 번 골라도 된다.

**제한 사항**

한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

# **💡 풀이 과정**

## 문제 접근

문제에서 같은 수를 여러 번 골라도 된다는 조건은, 숫자 자체를 반복해서 선택할 수 있다는 의미입니다. 따라서 탐색 과정에서 특정 숫자를 방문 처리하는 조건을 제거하면 같은 수를 여러 번 고를 수 있게 된다.

중복되는 수열을 여러 번 출력하는 것을 방지하기 위해 수열을 Set 이나 visit 배열을 통해 완성된 수열 값을 저장하고 확인하는 방법도 있다. 하지만 이 문제에서는 같은 숫자를 여러 번 고를 수 있는 조건이 자연스럽게 모든 경우의 수를 탐색하며 중복된 수열이 출력되지 않도록 보장한다. 따라서 별도의 자료구조를 사용하지 않아도 문제를 해결할 수 있다.

# ✏️ **풀이 코드**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static int N;
    public static int M;
    public static int[] arr;
    public static StringBuilder sb = new StringBuilder();

    public static void dfs(int depth) {

        if (depth == M) {
            for (int val : arr) {
                sb.append(val).append(' ');
            }
            sb.append('\n');
            return;
        }

        for (int i = 1; i <= N; i++) {
            arr[depth] = i;
            dfs(depth + 1);
        }
    }

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        arr = new int[M];

        dfs(0);
        System.out.println(sb);

    }
}
```

## 📋 주요 코드 설명

```java
 public static void dfs(int depth) {

      if (depth == M) {
          for (int val : arr) {
              sb.append(val).append(' ');
          }
          sb.append('\n');
          return;
      }

      for (int i = 1; i <= N; i++) {
          arr[depth] = i;
          dfs(depth + 1);
      }
  }
```

이렇게 해당 숫자가 사용되었는지 확인하는 조건이 없다면, 재귀 호출 과정에서 특정 깊이에서 선택한 숫자가 다음 깊이에서도 반복적으로 사용할 수 있다. 

# ✏️ **풀이 코드(Python)**

```python
import sys
input = sys.stdin.readline

def dfs(depth):
    if depth == M:
        print(" ".join(map(str, arr)))
        return

    for i in range(1, N + 1):
        arr[depth] = i
        dfs(depth + 1)

N, M = map(int, input().split())
arr = [0] * M

dfs(0)
```

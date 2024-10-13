# 1209. Sum[D3]

## 📋 주요 코드 설명(Java)

```java
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

try {
} catch (FileNotFoundException e) {
			// input.txt 파일을 읽기 위한 Scanner 생성
            Scanner sc = new Scanner(new File("input.txt"));
            // ....
            e.printStackTrace();
        }
```

Java에서 파일 입력을 처리할 때, Scanner를 File 객체와 함께 try 블록 안에서 생성하여 파일을 읽고, 파일이 존재하지 않을 경우 발생하는 FileNotFoundException을 catch 블록에서 처리한다.

# ✏️ **풀이 코드(Java)**

```java
import java.util.Scanner;
 
public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);  // 표준 입력으로 변경
        int T = 10;  // 테스트 케이스 개수
 
        for (int tc = 1; tc <= T; ++tc) {
            int t = sc.nextInt();  // 테스트 케이스 번호
            int N = 100;  // 배열 크기
            int[][] arr = new int[N][N];  // 배열 생성
 
            // 배열 입력 받기
            for (int i = 0; i < N; ++i) {
                for (int j = 0; j < N; ++j) {
                    arr[i][j] = sc.nextInt();
                }
            }
            int ans = 0;  // 최대값 저장
            int s3 = 0;   // 좌상-우하 대각선 합
            int s4 = 0;   // 우상-좌하 대각선 합
 
            // 행과 열의 합, 대각선 합 계산
            for (int i = 0; i < N; ++i) {
                int s1 = 0;  // 행의 합
                int s2 = 0;  // 열의 합
 
                for (int j = 0; j < N; ++j) {
                    s1 += arr[i][j];  // 행의 합
                    s2 += arr[j][i];  // 열의 합
                }
 
                // 최대값 갱신
                ans = Math.max(ans, Math.max(s1, s2));
 
                // 대각선 합 계산
                s3 += arr[i][i];  // 좌상-우하 대각선
                s4 += arr[i][N - i - 1];  // 우상-좌하 대각선
            }
            // 대각선 합과 행, 열 최대값 비교 후 갱신
            ans = Math.max(ans, Math.max(s3, s4));
            // 결과 출력
            System.out.println("#" + t + " " + ans);
        }

        sc.close();
    }
}
```

# 2805, 농작물 수확하기[D3]

# ✏️ **풀이 코드(Java)**

```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        // 테스트 케이스 수
        int T = sc.nextInt();
        
        // 여러 개의 테스트 케이스를 처리
        for (int tc = 1; tc <= T; tc++) {
            // 농장의 크기
            int N = sc.nextInt();
            // 농작물 입력
            int[][] arr = new int[N][N];
            for (int i = 0; i < N; i++) {
                // 각 농작물의 수 입력
                for (int j = 0; j < N; j++) {
                    arr[i][j] = sc.nextInt();
                }
            }
            // 농장의 중간
            int M = N / 2;
            // 총 농작물 수
            int ans = 0;
            // 열의 시작점, 열의 끝 초기화
            int s = M, e = M;

            // 농작물 수 계산
            for (int i = 0; i < N; i++) {
                for (int j = s; j <= e; j++) {
                    ans += arr[i][j];
                }
                if (i < M) {
                    s--; // 위쪽 부분
                    e++; // 위쪽 부분
                } else {
                    s++; // 아래쪽 부분
                    e--; // 아래쪽 부분
                }
            }
            // 결과 출력
            System.out.println("#" + tc + " " + ans);
        }
        // Scanner 종료
        sc.close();
    }
}
```

# 1225. 암호생성기[D3]

## 📋 주요 코드 설명(Java)

```java
 ArrayList<Integer> lst = new ArrayList<>();
```

Java에서는 타입을 인터페이스로 선언해야 구현체 값을 변경한다고 하면 유연하게 대처할 수 있다. 

하지만 이렇게 제출할 시 에러가 발생한다. 

정확한 이유는 모르겠지만, 메모리 효율성 때문으로 추측된다.

# ✏️ **풀이 코드**(Java)

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int T = 10;  // 테스트 케이스 개수

        for (int t = 1; t <= T; t++) {
            int tc = sc.nextInt();  // 테스트 케이스 번호
            ArrayList<Integer> lst = new ArrayList<>();
            // Java에서 리스트에 값 입력 받는 형식
            for (int i = 0; i < 8; i++) {
                lst.add(sc.nextInt());
            }
            int i = 1;
            while (true) {
                if (i > 5) {
                    i = 1;  // i가 5를 넘으면 1로 초기화
                }
                // 첫 번째 값을 꺼내서 감소시키기, remove 메서드는 인덱스의 값을 제거하고 반환
                int n = lst.remove(0) - i;

                if (n<= 0) {
                    lst.add(0);  // n이 0 이하이면 0을 추가하고 종료
                    break;
                }
                lst.add(n);  // 그렇지 않으면 감소한 값을 다시 리스트에 추가
                i +=1;
            }
            System.out.print("#" + tc); // print는 개행을 하지 않음
            for (int num : lst) {
                System.out.print(" " + num); // 리스트의 각 요소를 공백으로 구분되어 출력
            }
            System.out.println();
        }
        sc.close();
    }
}
```

# 1860. 진기의 최고급 붕어빵[D3]

## 📋 주요 코드 설명(Java)

```java
 for (int n : lst) {
      cnt +=1;  // 현재 처리 중인 사람 수 증가
      // 현재 시간이 얼마나 많은 붕어빵을 만들 수 있는지 계산
      if ((n / M) * K < cnt) {
          ans = "Impossible";  // 붕어빵을 충분히 만들 수 없을 경우
          break; 
      }
  }
```

사람이 도착하는 시간과  만드는데 걸리는 시간의 몫을 구하면, 손님이 도착한 시간 때 붕어의 생산량을 파악할 수 있고 붕어빵 생산량이 사람수 보다 적게 되면 Impossible을 반환한다.

그리고 for문을 순회하여 다음 도착하는 시간을 계산할 때에는 사람의 수를 증가시켜 그 다음 사람이 붕어빵을 먹는지 확인한다.

# ✏️ **풀이 코드**(Java)

```java
import java.util.Arrays;
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        
        int T = sc.nextInt();  // 테스트 케이스 수

        for (int tc = 1; tc <= T; tc++) {
            // 붕어빵 먹는 사람 수, 만드는데 걸리는 시간, 시간 당 만들 수 있는 갯수
            int N = sc.nextInt();  // 붕어빵을 먹는 사람 수
            int M = sc.nextInt();  // 만드는 데 걸리는 시간
            int K = sc.nextInt();  // 시간당 만들 수 있는 갯수

            // 각 사람이 언제 도착하는지 시간
            int[] lst = new int[N];
            for (int i = 0; i < N; i++) {
                lst[i] = sc.nextInt();  // 도착 시간 입력
            }

            // 붕어빵 사간 사람 수
            int cnt = 0;
            String ans = "Possible";  // 초기 상태는 가능
            Arrays.sort(lst);  // 도착 시간으로 정렬

            for (int n : lst) {
                cnt +=1;  // 현재 처리 중인 사람 수 증가
                // 현재 시간이 얼마나 많은 붕어빵을 만들 수 있는지 계산
                if ((n / M) * K < cnt) {
                    ans = "Impossible";  // 붕어빵을 충분히 만들 수 없을 경우
                    break; 
                }
            }
            System.out.printf("#%d %s\n", tc, ans);  
        }
        sc.close();  
    }
}

```

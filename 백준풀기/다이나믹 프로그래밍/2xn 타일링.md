### 문제
[2xn 타일링](https://www.acmicpc.net/problem/11726)를 풀자. <br>
+ 2×n 크기의 직사각형을 1×2, 2×1 타일로 채우는 방법의 수를 구하는 프로그램을 작성하시오.
+ 입력, 첫째 줄에 n이 주어진다. (1 ≤ n ≤ 1,000)
+ 출력, 2×n 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 구한다.

<br>

### 풀이 포인트
> [참고자료](https://kosaf04pyh.tistory.com/222)

+ 경우의 수를 생각해보자.
```
1x2 타일을 A, 2x2 타일을 B로 가정하고,

n=1 : [A]                          # A 1개 

n=2 : [AA, B]                      # B 1개

n = 3 : [AAA, AB, BA]              # A 1개, B 1개

n= 4 : [AAAA, ABA, BAA, AAB, BB]   # A 2개, B 1개 
```

<br>

 + 1x2 타일 (n-1)개와 2x2 타일 (n-2)개로 경우의 수를 구할 수 있다.
```
n = 3 -> A:2, B:1  result:3
n = 4 -> A:3, B:2  result:5
...
```

<br>

### 코드
```java
package hackerrank;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        int[] dp = new int[1001];
        dp[1] = 1;
        dp[2] = 2;

        for (int i=3; i<=n; i++) {
            dp[i] = (dp[i-1] + dp[i-2]) % 10007;
        }
        System.out.println(dp[n]);
    }
}
```

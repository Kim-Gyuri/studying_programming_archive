## 문제
+ If valid triangles having the maximum perimeter, Choose and print the one with the longest maximum side.
+ if no non-degenerate triangle exists, return -1.

### 포인트
`삼각형 조건` <br>
Given any triangle, if a, b, and c are the lengths of the sides, the following is always true: <br>
> a + b > c  <br> b + c > a <br> c + a > b

`어떻게 풀까?` <br>
+ a'라는 이름의 정렬 배열이 주어졌을 때(인덱스 x < y < z), a[x] <= a[y] <= a[z]가 항상 true.
+  a[x], a[y], a[z]가 세 변이고 그 순서대로 하나의 조건이 충족되는지 확인만 하면 된다.
+ 따라서 a[x] + a[y] > a[z]인 경우는 세 변을 출력, 아니면 -1을 출력한다.

`a[z]를 가장 큰 변으로 가정하면` <br>
+ 문제에서 consecutive indexed elements(연속된 인덱스)이면서 a[z] 변의 길이가 가장 길다고 했다.
+ 먼저 Arrays.sort(a) 정렬(오름차순)하고 for()문으로 검색한다.
+ a[i-3] + a[i-1] > a[i]인 경우는 세변을 출력한다.
+ a[i-3] + a[i-1] <= a[i]인 경우는 -1을 출력한다.

### 생각
제시된 문제의 코드를 변형해서 풀었는데, 내가 제출한 코드가 간단해서 맘에 들었다. <br>
+ maximumPerimeterTriangle(int n, int[] sticks)로 리턴값을 List<Integer>에서 void로 바꾸었다. 
+ 리스트로 받지 않고, for()문을 돌려 검색했을 때, 찾았을 때 printf로 간단하게 출력하고 return;한다.

<br>
 
빠른 입출력을 위해 StringTokenizer를 같이 사용했다. <br>
+ BufferedReader를 통해 읽어온 데이터는 개행문자 단위(Line 단위)로 나누어진다.
+ 만약 이를 공백 단위로 데이터를 가공하고자 하면 따로 작업을 해주어야 한다. 
+ 이때 사용하는 것이 StringTokenizer나 String.split() 함수이다.
+ StringTokenizer의 nextToken() 함수를 쓰면, readLine()을 통해 입력 받은 값을 공백 단위로 구분하여 순서대로 호출할 수 있다.
  
> [참고 코드](https://www.hackerrank.com/challenges/maximum-perimeter-triangle/forum/comments/192051)

## 코드
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");

        int n = Integer.parseInt(st.nextToken());
        
        st = new StringTokenizer(br.readLine(), " ");
        int[] sticks = new int[n];
        for (int i=0; i<n; i++) {
            sticks[i] = Integer.parseInt(st.nextToken());
        }

        maximumPerimeterTriangle(n, sticks);
    }
    
     private static void maximumPerimeterTriangle(int n, int[] sticks) {
         Arrays.sort(sticks);
         int p1, p2, p3;
         for (p1=n-3,p2=n-2,p3=n-1; sticks[p1]+sticks[p2]<=sticks[p3]; p1--,p2--,p3--) {
             if (p1==0) {
                 System.out.print(-1);
                 return;
             }
         }
         System.out.printf("%d %d %d", sticks[p1], sticks[p2], sticks[p3]);
     }
}
```

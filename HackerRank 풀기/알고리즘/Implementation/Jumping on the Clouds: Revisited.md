## 문제
+ 구름정보 배열을 입력으로 받는다.
+ There is an array of clouds, c[i]
+ the energy level is 100.
+ if c[i] = 1, cloud is a thunderhead. (소나기 구름)
+ if c[i] = 0, cloud is a cumulus cloud.
+ 구름이 바뀔 때 1씩 에너지가 소비되지만, thunderhead으로 바뀌는 경우는 추가로 에너지 2를 소비한다.
+ output : the energy level remaining.

### Sample
```
# Sample Input
8 2             
0 0 1 0 0 1 1 0 



# Sample Output
92



# Explanation
n = 8, k = 2                    #8개 구름을 2칸씩 이동한다.              
c = [0, 0, 1, 0, 0, 1, 1, 0]


sequence of moves:
i=0 -> i=2 일 때, 100-1-2 = 97   #0->1로 바뀌니 2를 더 소비한다.
i=2 -> i=4 일 때, 97-1 = 96
i=4 -> i=6 일 때, 96-1-2 = 93    #0->1로 바뀌니 2를 더 소비한다.
i=6 -> i=0 일 때, 93-1 = 92

최종 에너지는 92가 된다.
```


## 풀이
```
# 코드 스케치
energy = 100;                                                     #에너지 초기값은 100
for (c[i] 구름을 돌면서, 2칸씩 jump해야 한다.) {
  energy = (jump했을 때 thunderhead이면 -3, cumulus cloud이면 -1); #삼항연산자로 구현
  if (점프했을 때 i값이 n보다 크면) i -= c.length;                  #c.length를 빼서 순회단위를 c.length 크기로 맞춘다.
}
return energy;





# 제출한 코드
    static int jumpingOnClouds(int[] c, int k) {
        int energy = 100;
        for (int i=0; i<c.length; i+=k) {
            energy -= c[i] == 0 ? 1 : 3;
            if (i+k > c.length) i-= c.length;
        }
        return energy;
    }
```


## 코드
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {
    // Complete the jumpingOnClouds function below.
    static int jumpingOnClouds(int[] c, int k) {
        int energy = 100;
        for (int i=0; i<c.length; i+=k) {
            energy -= c[i] == 0 ? 1 : 3;
            if (i+k > c.length) i-= c.length;
        }
        return energy;

    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nk = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nk[0]);

        int k = Integer.parseInt(nk[1]);

        int[] c = new int[n];

        String[] cItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            int cItem = Integer.parseInt(cItems[i]);
            c[i] = cItem;
        }

        int result = jumpingOnClouds(c, k);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```

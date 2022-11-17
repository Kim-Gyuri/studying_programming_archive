## 문제
+ input 1 Line  : q (You are given  q queries in the form)
+ input 2 Line  : x y z (순서대로 Cat_A's location, cat_B's location, mouse_C's location)
+ output : What's at q Query position?

### Sample
```
# Sample Input 0
2
1 2 3
1 3 2


# Sample Output 0
Cat B
Mouse C


# Explanation 0
query 0 : cat_A, cat_B, mouse_C
query 1 : cat_A, mouse_C, cat_B
->> 2번째에는 각각 cat_B, mouse_C가 있다.
```

## 풀이
```
# 코드 스케치
경우의 수는 3가지
1) cat_A가 먼저 mouse_C를 잡는지?
2) cat_B가 먼저 mouse_C를 잡는지?
catA - catB - mouseC    # 1 2 3
catB - catA - mouseC    # 2 1 3
# 마우스와 거리는 Math.abs()로 절대값으로 계산해준다.
# 마우스와 거리가 가까울 수록 먼저 잡는다.
# catA-mouseC 거리 < catB-mouseC 거리 인 경우에는 catA가 가까워서 "cat A" 
# catA-mouseC 거리 > catB-mouseC 거리 인 경우에는 catB가 가까워서 "cat B" 
# 삼항연산자로 처리한다, |catA-mouseC| < |catB-mouseC| ? "cat A" : "Cat B"



3) 동시에 mouse_C를 잡는지?
catA - mouseC - catB      # 1 3 2
catB - mouseC - catA      # 3 1 2
# 이 경우 return "Mouse C"이어야 한다. 
# mouseC는 항상 2에 있어야 한다. 
# cat - mouse 값이 항상 (1,2) (3,2)이면 되므로 if (|1-2| == |3-2|) return 쥐탈출


# 제출한 코드
        if (Math.abs(x-z) == Math.abs(y-z)) return "Mouse C";
        return Math.abs(x-z) < Math.abs(y-z) ? "Cat A" : "Cat B";
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

    // Complete the catAndMouse function below.
    static String catAndMouse(int x, int y, int z) {
        if (Math.abs(x-z) == Math.abs(y-z)) return "Mouse C";
        return Math.abs(x-z) < Math.abs(y-z) ? "Cat A" : "Cat B";
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int qItr = 0; qItr < q; qItr++) {
            String[] xyz = scanner.nextLine().split(" ");

            int x = Integer.parseInt(xyz[0]);

            int y = Integer.parseInt(xyz[1]);

            int z = Integer.parseInt(xyz[2]);

            String result = catAndMouse(x, y, z);

            bufferedWriter.write(result);
            bufferedWriter.newLine();
        }

        bufferedWriter.close();

        scanner.close();
    }
}
```

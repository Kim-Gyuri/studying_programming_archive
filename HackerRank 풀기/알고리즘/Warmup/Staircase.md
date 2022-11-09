## 문제
일명 오른쪽 아래가 직각인 삼각형을 #으로 찍기 (단, 밑변의 길이를 입력받는다.)

## 풀이
```
공백: _  

_ _ _ _ _ #
_ _ _ _ # #
_ _ _ # # #
_ _ # # # #
_ # # # # #
# # # # # #

    for (int i=1; i<n+1; i++) {
        for (int j=n; j>i; j--) {
            System.out.print(" "); 공백넣고
        }
        for (int j=0; j<i; j++) {
            System.out.print("#"); # 찍고
        }
        System.out.println(); 줄 바꾸고
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
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;

class Result {

    /*
     * Complete the 'staircase' function below.
     *
     * The function accepts INTEGER n as parameter.
     */

    public static void staircase(int n) {
    // Write your code here
    for (int i=1; i<n+1; i++) {
        for (int j=n; j>i; j--) {
            System.out.print(" ");
        }
        for (int j=0; j<i; j++) {
            System.out.print("#");
        }
        System.out.println();
    }
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        Result.staircase(n);

        bufferedReader.close();
    }
}
```

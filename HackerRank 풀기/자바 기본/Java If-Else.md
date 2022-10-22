## 출제 포인트
if, else를 사용할 수 있는지

## 문제 포인트
+ 홀수 -> Weird
+ 짝수,2to5 -> Not Weird
+ 짝수, 6to20 -> Weird
+ 짝수, n>20 -> Not Weird

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



    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int N = scanner.nextInt();
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");
        
        if (N%2==0) {
            if (N>=2 && N<=5) {
                System.out.println("Not Weird");
            } else if (N>=6 && N<=20) {
                System.out.println("Weird");
            } else if (N>20) {
                System.out.println("Not Weird");
            }
        } else {
            System.out.println("Weird");
        }
        scanner.close();
    }
}
```

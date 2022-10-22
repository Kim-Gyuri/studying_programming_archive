## 출제 포인트
자바 String의 compareTo(), toUpperCase, subString()을 알고 있는지? <br>
+ 문자 길이 a.length()
+ 길이 비교는 compareTo()로 비교한다. (기준값이 크면 1, 동일값은 0, 기준값이 작으면 -1를 반환한다.)
+ subString(i)는 인덱스 i부터 호출한다.
+ subString(a,b)는 인덱스a부터 b-1까지 호출한다.

> [subString() 참고사이트](https://jamesdreaming.tistory.com/81) <br>
> [compareTo() 참고사이트](https://mine-it-record.tistory.com/133) 

## 문제
+ 1줄에 a+b 문자열길이
+ 2줄에 a와 b 길이비교 후, a가 더 크면 YES를 반환
+ 3줄에 첫글자를 대문자로 바꿔 a+b를 출력

## 코드
```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        
        Scanner sc=new Scanner(System.in);
        String A=sc.next();
        String B=sc.next();
        /* Enter your code here. Print output to STDOUT. */
        System.out.println(A.length()+B.length());
        System.out.println(A.compareTo(B) > 0 ? "Yes" : "No");
        System.out.println(A.substring(0,1).toUpperCase() + A.substring(1) + " " + B.substring(0,1).toUpperCase() + B.substring(1));
        
    }
}
```





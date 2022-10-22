## 포인트
`Java - printf()로 문자열 포맷 출력할 수 있는가?` <br>
+ System.out.printf(format, arguments)
> %n : 줄바꿈 <br>
%s : String 형식으로 출력 <br>
%d : integer 형식으로 출력 <br>
%f : float 형식으로 출력 <br>
%t : date, time 형식으로 출력 <br>
%o : 8진수 integer 형식으로 출력 <br>
%x : 16진수 integer 형식으로 출력 <br>
%b : boolean 형식으로 출력 <br>
%e : 지수 형식으로 출력 <br>

> [참고 사이트](https://codechacha.com/ko/java-printf-format/) 

## 문제 포인트
+ 첫번째 열에 15자를 제한한다. ("%-15s", s1)
+ 두번째 열에 Integer 3digit를 제한한다. ("%03d%n", x)

## 코드
```java
import java.util.Scanner;

public class Solution {

    public static void main(String[] args) {
            Scanner sc=new Scanner(System.in);
            System.out.println("================================");
            for(int i=0;i<3;i++)
            {
                String s1=sc.next();
                int x=sc.nextInt();
                //Complete this line
                System.out.printf("%-15s", s1);
                System.out.printf("%03d%n", x);
            }
            System.out.println("================================");

    }
}
```

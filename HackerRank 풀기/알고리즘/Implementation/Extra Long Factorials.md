## 문제
+ 팩토리얼 계산결과를 출력한다.
+ 포인트: can handle big integers


### Sample 
```
# Input Format
25


# Output Format
15511210043330985984000000



# Explanation
25 ! = 25 x 24 x 23 ... x 1
```

## 풀이
```
# BigInteger
1) 문자열 형태로 이루어져 있어 숫자의 범위가 무한하기에 어떠한 숫자이든지 담을 수 있습니다.
2) 문자열이기에 사칙연산이 안됩니다. 
3) 그렇기에 BigIntger 내부의 숫자를 계산하기 위해서는 BigIntger 클래스 내부에 있는 메서드를 사용해야 합니다.

# BigInteger 계산
BigInteger bigNumber1 = new BigInteger("100000");
BigInteger bigNumber2 = new BigInteger("10000");
System.out.println("곱셈(*) :" +bigNumber1.multiply(bigNumber2));



# 코드 스케치
팩토리얼 계산할 때, n! = n ...1까지 곱해야 한다.  (그래서 bi를 1로 선언, 1~n까지 곱셈)

bi = new BinInteger("1");   # 1~n까지 곱해야 하므로 1을 초기값으로 선언한다.
for (1~n까지 돌면서) {
   bi = bi.multiplay(new BigInteger("" + i));  #곱셈
}
print(bi);




# 제출한 코드
        BigInteger bi = new BigInteger("1");
        for (int i=1; i<=n; i++) {
            bi = bi.multiply(new BigInteger(""+i));
        }
        System.out.println(bi);
```

> 참고 사이트 <br>
> [큰 정수 다루기](https://coding-factory.tistory.com/604) <br>


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
     * Complete the 'extraLongFactorials' function below.
     *
     * The function accepts INTEGER n as parameter.
     */

    public static void extraLongFactorials(int n) {
    // Write your code here
        BigInteger bi = new BigInteger("1");
        for (int i=1; i<=n; i++) {
            bi = bi.multiply(new BigInteger(""+i));
        }
        System.out.println(bi);
    }
   

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        Result.extraLongFactorials(n);

        bufferedReader.close();
    }
}
```

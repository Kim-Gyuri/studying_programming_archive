## 문제
+ 주어진 문자열의 숫자를 k번 더한 수를 superDigit()를 재귀호출하여 1자리가 될 때까지 반복한다.

<br>

### 포인트
+ 문자열의 길이가 1이면 (재귀) 구할 필요가 없다. 바로 리턴
+ 각 문자열의 숫자를 합하고 K를 곱한다.
+ 문자열의 길이가 1이 될 때까지 (재귀호출  ->) 각 문자열의 숫자를 합하여 super digit를 구한다.

## 풀이
```
# 코드 스케치
call (String s) {  #call() :문자열을 digit로 바꿔 각 자리 digit의 sum을 구한다.
문자열 길이가 1인 경우 -> Integer.parseInt(s); 그대로 형변환
문자열 길이 2이상      -> s += Integer.parseInt(String.valueOf(s.charAt(i))); charArray로 변환-> int 변환 -> sum구하기
}

superDigit(String s, int k) {
  int num = 문자열 숫자 합 * k;
  while (num이 2자리인 경우) {
    num = call(String.valueOf(num)); #재귀호출을 계속해준다.
  }
  return num;






# 제출한 코드 
    public static int superDigit(String n, int k) {
    // Write your code here
        int num = call(n)*k;
        while(num > 9) {
            num = call(String.valueOf(num));
        }   
        return num;
    }
      
    public static int call(String s) {
        while(s.length() != 1) {
            int sum = 0;
            for (int i=0; i< s.length(); i++) {
                sum += Integer.parseInt(String.valueOf(s.charAt(i)));
            }
            s = String.valueOf(sum);
        }
        return Integer.parseInt(s);
    }

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
     * Complete the 'superDigit' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. STRING n
     *  2. INTEGER k
     */

    public static int superDigit(String n, int k) {
    // Write your code here
        int num = call(n)*k;
        while(num > 9) {
            num = call(String.valueOf(num));
        }   
        return num;
    }
    
    public static int call(String s) {
        while(s.length() != 1) {
            int sum = 0;
            for (int i=0; i< s.length(); i++) {
                sum += Integer.parseInt(String.valueOf(s.charAt(i)));
            }
            s = String.valueOf(sum);
        }
        return Integer.parseInt(s);
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        String n = firstMultipleInput[0];

        int k = Integer.parseInt(firstMultipleInput[1]);

        int result = Result.superDigit(n, k);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

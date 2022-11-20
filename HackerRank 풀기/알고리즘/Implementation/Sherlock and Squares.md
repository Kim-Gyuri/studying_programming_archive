## 문제
+ input 1 Line : q, the number of test cases.
+ input 2 Line : a b, the starting and ending integers in the ranges.
+ output : the number of square integers in the range.


### Sample
```
# Sample Input
2
3 9
17 24


# Sample Output
2
0



# Explanation
[3,9] range 
4 -> 2x2
9 -> 3x3
2가지가 된다.

[17,24] range
없다.
```

## 풀이
```
# 코드 스케치
Math. floor() 소수점 이하 버리기
Math.sqrt() 제곱근 구하기
Math.ceil() 소수점 이하 올리기

[3,9] range 
4 -> 2x2
9 -> 3x3
예시처럼 제곱근 사이의 정수 개수가 the number of square integers in the range가 되므로
Math.sqrt(b) - Math.sqrt(a) + 1를 해주면 된다. 
오차가 있을 수 있으니 아래처럼 형변환도 해준다.





# 제출한 코드
       return (int) (Math.floor(Math.sqrt(b)) - Math.ceil(Math.sqrt(a)) + 1);
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
     * Complete the 'squares' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER a
     *  2. INTEGER b
     */

    public static int squares(int a, int b) {
    // Write your code here
       return (int) (Math.floor(Math.sqrt(b)) - Math.ceil(Math.sqrt(a)) + 1);
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int q = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, q).forEach(qItr -> {
            try {
                String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

                int a = Integer.parseInt(firstMultipleInput[0]);

                int b = Integer.parseInt(firstMultipleInput[1]);

                int result = Result.squares(a, b);

                bufferedWriter.write(String.valueOf(result));
                bufferedWriter.newLine();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

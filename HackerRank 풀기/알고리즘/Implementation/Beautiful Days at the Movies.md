## 문제
+ 1줄에 3개의 숫자가 주어지는데, 나누었을 때 정수인 경우의 수를 출력한다.
+ int i: the starting day number
+ int j: the ending day number
+ int k: the divisor

### Sample
```
# Sample Input
20 23 6


# Sample Output
2


# Explanation
20~23일까지 계산해보는데
Day 20 : |20-02| /6 = 3
Day 21 : |21-12| /6 = 1.5
Day 22 : |22-22| /6 = 0
Day 23 : |23-32| /6 = 1.5

정수인 수 3,0인 Day[20,22]이 된다. (총 2개)
```


## 풀이
```
# 코드 스케치
beautifulDays = 0;   #조건에 맞는 날짜 개수를 저장할 변수 선언
for (주어진 기간동안) {
   reverse_day = 해당 날짜 숫자를 뒤집은 것;
   # reverse_ day 만들기 ->
   # int -> String으로 변형,  StringBuffer.reverse()..로 문자열 뒤집고 , 다시 int로 형변형한다.
   
   if (|(day - reverse_day)| % k == 0) beautifutDays++; 
   # 나머지가 없으면 정수이므로 if()문으로 조건을 걸어준다.
}
return beautifulDays;




# 제출한 코드
        int beautifulNum = 0;
        for (int day=i; day<=j; day++) {
            String s = String.valueOf(day);
            StringBuffer sb = new StringBuffer(s);
            int reverse_day = Integer.valueOf(sb.reverse().toString());
            
            if (Math.abs(day-reverse_day)%k == 0) beautifulNum++;
        }
        return beautifulNum;
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
     * Complete the 'beautifulDays' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER i
     *  2. INTEGER j
     *  3. INTEGER k
     */

  public static int beautifulDays(int i, int j, int k) {
    // Write your code here
        int beautifulNum = 0;
        for (int day=i; day<=j; day++) {
            String s = String.valueOf(day);
            StringBuffer sb = new StringBuffer(s);
            int reverse_day = Integer.valueOf(sb.reverse().toString());
            
            if (Math.abs(day-reverse_day)%k == 0) beautifulNum++;
        }
        return beautifulNum;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int i = Integer.parseInt(firstMultipleInput[0]);

        int j = Integer.parseInt(firstMultipleInput[1]);

        int k = Integer.parseInt(firstMultipleInput[2]);

        int result = Result.beautifulDays(i, j, k);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

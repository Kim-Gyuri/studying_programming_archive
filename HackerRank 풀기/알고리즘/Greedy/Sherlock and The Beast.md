## 문제
+ Its digits can only be 3's and/or 5's.           (digit는 3과 5로만 이루어진다.)
+ The number of 3's it contains is divisible by 5. (digit가 5의 배수인 경우 3으로 채운다.)
+ The number of 5's it contains is divisible by 3. (digit가 3의 배수인 경우 5로 채운다.)
+ It is the largest such number for its length. (최대한 길이 길게 출력한다.)

### 예시
```
# Sample Input
STDIN   #Function
-----   #--------
4       # t = 4
1       # n = 1  (first test case)
3       # n = 3  (second test case)
5       # n = 5  (third test case)
11      # n = 11 (fourth test case)



# Sample Output
-1
555
33333
55555533333
```

###  예시 문제설명
```
(1) n = 1 경우
n은 5,3의 배수가 아니다. 해당이 안되므로 return -1


(2) n = 3 경우
n은 3의 배수다. (3*1) return 555


(3) n = 5 경우
n은 5의 배수다. (5*1) return 33333


(4) n = 11 경우
답을 보면 "5"가 먼저 채워진다. -> 그래서 먼저 3으로 나눠봐야 한다.
n=11 n을 먼저 3으로 나눈다. n%3=2
"목적:길이 길게-> 최대한 3을 넣어야 한다."
"33333"을 넣기 위해서는 n=5 만큼을 남겨둬야 한다. (그래서 n-3을 해서 keep해둔다.)
((n-3)/3) 몫 만큼 "555"를 채운다. --> 몫:2 "555555"
return "555555" + "333" 
```


## 풀이
+ 우선 n%3 했을 때, 나머지를 확인한다. (왜냐하면 "555"를 경우를 우선체크해야 함)
```
n = 3 -> "555"
n = 5 -> "33333"

n = (3*2) = 6             #n%3=0
n = (3*1) + (5*1) = 8     #n%3=2
n = (3*3)         = 9     #n%3=0
n = (5*2)         = 10    #n%3=1
n = (3*2) + (5*1) = 11    #n%3=2
n = (3*4)         = 12    #n%3=0
n = (3*1) + (5*2) = 13    #n%3=1
n = (3*3) + (5*1) = 14    #n%3=2
n = (3*2) + (5*2) = 16    #n%3=1
n = (3*4) + (5*1) = 17    #n%3=2
n = (3*5) + (5*1) = 20    #n%3=2 
n = (3*2) + (5*3) = 21    #n%3=0
n = (3*3) + (5*3) = 24    #n*3=0



# 나머지 경우 수를 switch()로 판단한다.
-> 나머지가 0,1,2로 나누어 판단하면 된다.

(1) 나머지 = 0 인 경우
n = (3*2) = 6             #n%3=0
n = (3*3) = 9             #n%3=0
n = (3*4) = 12            #n%3=0
n/3 몫만큼 "555"를 채워 반환한다.


(2) 나머지 = 1 인 경우
n = (5*2) = 10            #n%3=1
n = (3*1) + (5*2) = 13    #n%3=1
n = (3*2) + (5*2) = 16    #n%3=1
if (n-10 ≥ 0)를 기준으로 나누면 되는데,
n = 10부터 (5*2)에서 (3*i)를 해주면 된다.
if (n-10 >= 0) {
  String fillLengthTes = BRICK_DECENT_THREES + BRICK_DECENT_THREES;
  decentNum = repeat(BRICK_DECENT_FIVES, ((n-10)/3)) + fillLengthTes;




(3) 나머지 = 2 인 경우 
n = 2는 해당이 안되는 숫자 (3/5가 안 됨) -> 그래서  n-3>=0인지 먼저 확인해야 한다.

n = 5                     #n%3=2
n = (3*1) + (5*1) = 8     #n%3=2  
n = (3*2) + (5*1) = 11    #n%3=2  
n = (3*3) + (5*1) = 14    #n%3=2  
n = (3*4) + (5*1) = 17    #n%3=2  
n = (3*5) + (5*1) = 20    #n%3=2  
5가 필수이므로 (n에서 5를 킵해둔다,-> 나머지(2), n에서 3)
그리고 (n-3)/3을 계산하면 3의 반복수를 구할 수 있다.
if (n-3 >= 0) {
decentNum = repeat(BRICK_DECENT_FIVES, ((n-3)/3)) + BRICK_DECENT_THREES;
```

<br>

+ repeat() 메소드 
```
3의 반복수(times)만큼 해당 "555"수를 구한다.
    private static String repeat(String value, int times) {
        return new String(new char[times]).replace("\0", value);
    }
```

> [ Repeat string using regex 참고 사이트](https://howtodoinjava.com/java11/repeat-string-n-times/) <br>


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
    private static final String BRICK_DECENT_FIVES = "555";
    private static final String BRICK_DECENT_THREES = "33333";   
    
    public static void decentNumber(int n) {
    // Write your code here
        String decentNum = "-1";
        switch(n % 3) {
            case 0:
                decentNum = repeat(BRICK_DECENT_FIVES, n/3);
                break;
            case 1:
                if (n-10 >= 0) {
                    String fillLengthTes = BRICK_DECENT_THREES + BRICK_DECENT_THREES;
                    decentNum = repeat(BRICK_DECENT_FIVES, ((n-10)/3)) + fillLengthTes;
                }
                break;
            case 2:
                if (n-3 >= 0) {
                    decentNum = repeat(BRICK_DECENT_FIVES, ((n-3)/3)) + BRICK_DECENT_THREES;
                    break;
                }                  
        }
        System.out.println(decentNum);
    }
    
    private static String repeat(String value, int times) {
        return new String(new char[times]).replace("\0", value);
    }
}

public class Solution {

    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int t = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, t).forEach(tItr -> {
            try {
                int n = Integer.parseInt(bufferedReader.readLine().trim());

                Result.decentNumber(n);
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        bufferedReader.close();
    }
    
}
```

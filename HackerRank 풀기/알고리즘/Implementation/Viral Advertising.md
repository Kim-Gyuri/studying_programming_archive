## 문제
+ they advertise it to exactly 5 people on social media. (1Day, 5 shared)
+ On the first day, half of those 5 people like the advertise ( 1Day, 2 liked)
+ At the beginning of the second day, (1 day Liked * 3) people receive the advertisement. (2Day, 6 shared)
+ input  : the day number.
+ output : we return the cumulative number of likes.

## 풀이
```
# 코드 스케치
share = 5;       #처음 share 5부터 시작한다.
cumulative = 0;  #축적된 liked 수를 저장할 변수 선언
while (n 날짜만큼 도는데) {
  liked = share/2;        #liked는 share의 절반 정수값이 된다.
  cumulative += lkied;    #liked 수를 축적한다.
  share = liked*3;        #liked * 3를 해서, 다음날 share를 구한다. 
}

return cumulative;        #결과를 반환






# 제출한 코드
        int share = 5;
        int cumulative = 0;
        while(n-- >0) {
            cumulative += share/2;
            share = share/2*3;
        }
        return cumulative;

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
     * Complete the 'viralAdvertising' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER n as parameter.
     */

    public static int viralAdvertising(int n) {
    // Write your code here
        int share = 5;
        int cumulative = 0;
        while(n-- >0) {
            cumulative += share/2;
            share = share/2*3;
        }
        return cumulative;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        int result = Result.viralAdvertising(n);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

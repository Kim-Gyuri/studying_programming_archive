## 문제
+ 입력 n: 책의 총 페이지 수 
+ 입력 p: 돌릴 페이지 번호
+ 출력  : 반환할 최소 페이지 수
+ (단, 1페이지는 항상 오른쪽 페이지로 구성된다.)

### Sample
```
# Sample Input 0
6
2

# Sample Output 0
1



# Explanation 0
( ,1) -> (2,3)       #1 turn
( ,6) -> (5,4) -> (3,2) #2 turn
가장 최소한 페이지는 1이므로, 1을 출력한다.
---------------------------------------

# Sample Input 1
5
4


# Sample Output 1
0


# Explanation 1
(4, 5)                     # they do not need to turn any pages : 0 pages
( 1) (2,3) (4,5)           # they need to turn 2 pages
```

## 풀이
```
# 아이디어
(1) 총 페이지가 홀수인 경우를 고려한다.
n = 5인 경우
( ,1) (2,3) (4,5)이 된다. 
n - p + 1 (#왜냐하면, 페이지 1은 항상 오른쪽 페이지로 존재해야 하므로 1을 더해준다.)



(2) 총 페이지가 짝수인 경우를 고려한다.
n = 6인 경우
( ,1) (2,3) (4,5) (6, )
위처럼 짝수인 경우 페이지 1개가 더 필요하다. 



(3)n = 짝수/홀수인지 판단
짝수/홀수인지 알기 위해 n % 2를 한다. (홀수면 나머지 1, 짝수면 나머지 0) 
짝수 -> 1 - (n%2= 0)를 해줘서 n - p + 1을 추가한다.
홀수 -> 1 - (n%2= 1)를 해줘서 n - p 만 계산해준다.



(4) 책을 펼치면 왼쪽 오른쪽 2페이지가 존재하는데,
    주어진 페이지 번호/2를 해주면 그 페이지가 몇번째 장인지 알 수 있다.
    return Math.min(p, (n-p+1-(n%2)))/2;
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
     * Complete the 'pageCount' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER n
     *  2. INTEGER p
     */

    public static int pageCount(int n, int p) {
    // Write your code here
        return Math.min(p, (n-p+1-(n%2)))/2;
    }
    

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        int p = Integer.parseInt(bufferedReader.readLine().trim());

        int result = Result.pageCount(n, p);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

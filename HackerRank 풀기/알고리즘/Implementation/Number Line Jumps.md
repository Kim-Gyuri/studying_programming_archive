## 문제
+ 같은 시간에 같은 위치로 만날 수 있으면 YES, 아니면 NO를 출력한다.
+ 입력 K1의 처음 위치, 속력, K2의 처음 위치 속력을 받는다.
### Sample
```
# Sample Input 0
0 3 4 2

# Sample Output 0
YES

# Explanation 0
K1 : (0->3) (3->6) (6->9) (9->12)
K2 : (4->6) (6->8) (8->10) (10->12)

4번의 점프를 했을 때 동시에 K1, K2가 만나게 된다. -> YES
K2 starting location >  K1 starting location 
K2 rate < K1 rate


# Sample Input 1
0 2 5 3

# Sample Output 1
NO

# Explanation 1
K2 starting location >  K1 starting location 
K2 rate > K1 rate
K2가 이미 앞에 있어서, K1가 절대 따라잡을 수 없다.
```


## 풀이
+ 위의 예시처럼, 한쪽의 시작점과 속력만 크다면 결과는 무조건 NO가 된다.
```
경우 1  #if()문이 false일 때
K2 starting location >  K1 starting location 
K2 rate < K1 rate

경우 2 #if()문이 true일 때
K2 starting location <  K1 starting location 
K2 rate > K1 rate

2가지 경우를 모두 만족했을 때, 같은 시간대에 만나는지 판단하면 된다.
if ((x1>=x2) == (v2>=v1))
```

<br> <br>


+ 해당 식의 값이 0인 경우, 점프 횟수 j는 정수가 된다.
```
(x2−x1)mod(v1−v2)==0

if (Math.abs(x2-x1)%Math.abs(v2-v1)==0) result="YES";
```

> [참고 사이트](https://studyalgorithms.com/misc/hackerrank-number-line-jumps/) <br>

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
     * Complete the 'kangaroo' function below.
     *
     * The function is expected to return a STRING.
     * The function accepts following parameters:
     *  1. INTEGER x1
     *  2. INTEGER v1
     *  3. INTEGER x2
     *  4. INTEGER v2
     */

    public static String kangaroo(int x1, int v1, int x2, int v2) {
        // Write your code here
        String result = "NO";
        if ((x1>=x2) == (v2>=v1)) {
            if (Math.abs(x2-x1)%Math.abs(v2-v1)==0) result="YES";
        }
        return result;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int x1 = Integer.parseInt(firstMultipleInput[0]);

        int v1 = Integer.parseInt(firstMultipleInput[1]);

        int x2 = Integer.parseInt(firstMultipleInput[2]);

        int v2 = Integer.parseInt(firstMultipleInput[3]);

        String result = Result.kangaroo(x1, v1, x2, v2);

        bufferedWriter.write(result);
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

## 문제
+ Utopian Tree는 1년에 2번 성장을 한다.
+ 봄에는 2배로, 여름에는 +1만큼 성장한다.
+ input 1 Line   : the number of test cases.
+ input 2 Line ~ :  Period

### Sample
```
# Sample Input
3
0
1
4



# Sample Output
1
2
7




# Explanation
n = 3이므로 3개의 분기의 성장을 출력한다.
h를 나무 높이라고 했을 때,
period = 0 경우, h = 1 초기값이 된다.
period = 1 경우, (봄이므로 2배)  h = 2 * h = 2
period = 2 경우, (여름이므로 +1) h = h + 1 = 3
period = 3 경우, (봄이므로 2배)  h = 2 * h = 6
period = 4 경우, (여름이므로 +1) h = h + 1 = 7

순서대로 
0 ->1
1 ->2
4 ->7
확인할 수 있다.
``` 

## 풀이
```
# 코드 스케치
봄 - 여름을 짝수/홀수로 구분하면 된다.
i % 2 했을 때 나머지가 1인지 0인지 판단한다.
나머지 1인 경우,  봄    -> 홀수   -> 2배
나머지 0인 경우,  여름  -> 짝수   -> +1

h = 0;   #높이를 선언
for (n 분기까지 돌면서) {
    i%2 == 0 -> h = h+1;  #여기 판단을 삼항연산자로 구현
    i%2 != 0 -> h = h*2;
}
return h;





# 제출한 코드
        int height = 0;
        for (int i=0; i<=n; i++) {
           height = i%2!=0 ? height*2 : height+1;
        }
        return height;
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
     * Complete the 'utopianTree' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER n as parameter.
     */
    public static int utopianTree(int n) {
    // Write your code here
        int height = 0;
        for (int i=0; i<=n; i++) {
           height = i%2!=0 ? height*2 : height+1;
        }
        return height;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, t).forEach(tItr -> {
            try {
                int n = Integer.parseInt(bufferedReader.readLine().trim());

                int result = Result.utopianTree(n);

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




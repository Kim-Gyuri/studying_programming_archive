## 문제
+ 필요한 복용량을 출력한다.
+ magic potion을 먹으면 높이 1를 더 높게 뛸 수 있다.
+ input 1 line : n k (전체 허들 수, 가뿐히 뛸 수 있는 허틀 높이)
+ input 2 line : n가지의 허틀 높이 

### Sample 
```
# Sample Input 0
5 4
1 6 3 5 2


# Sample Output 0
2


# Explanation
Dan은 가뿐히 높이 4를 넘을 수 있다.
이때 가장 높은 허들은 6이므로,  6-4=2만큼 복용해야 한다.
```

## 풀이
```
# 코드 스케치
result = 0 #현재 가장 높은 허들을 저장할 변수 선언
for (전체 허들을 돌면서) {
  if (k보다 높은 허들이면서, 이전 허들보다 높은 경우)  result = 현재 허들
  else (n개의 허들보다, 높이 k인 허들이 가장 높은 경우) result = 0; 
  
  
  result = 0 -> return 0;             #가장 높은 허들이 k인 경우, 모두 넘을 수 있으므로 복용량은 0이다.
  result = n[i] -> return n[i] - h;   #가장 높은 허들이 k가 아닌 경우, n[i]-k만큼 복용해야 한다.





# 작성한 코드
        int result = 0;
        for (Integer hurdle : height) {
            if (k<hurdle && hurdle>result) result = hurdle;
        }
        return result == 0 ? 0 : result-k;
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
     * Complete the 'hurdleRace' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER k
     *  2. INTEGER_ARRAY height
     */

    public static int hurdleRace(int k, List<Integer> height) {
    // Write your code here
        int result = 0;
        for (Integer hurdle : height) {
            if (k<hurdle && hurdle>result) result = hurdle;
        }
        return result == 0 ? 0 : result-k;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> height = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.hurdleRace(k, height);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

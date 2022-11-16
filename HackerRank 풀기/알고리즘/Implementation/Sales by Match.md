## 문제
각 양말의 색상을 나타내는 정수 배열이 주어지면 일치하는 색상의 양말이 몇 쌍인지 출력한다.

### Sample
```
# Sample Input                     
9                          
10 20 20 10 10 30 50 10 20 


# Sample Output
3



# Explanation
n = 9
ar = [10, 20, 20, 10, 10, 30, 50, 10, 20]
pair 1(10,10,10)
pair 2(20,20,20)
not match (30,50)
```

## 풀이
+ 오름차순으로 정렬한 array에서, if()문으로 연속으로 같은 값이 있는 경우를 카운팅한다.

```
# 코드 스케치
int pair = 0; #짝이 있는 경우의 수를 선언
Collections.sort(ar); #오름차순 정렬
for (ar를 돌면서) {
  if (ar[i] == ar.[i+1]) #연속으로 같다면 pair 수를 증가시킨다.
  pair++;
  i++;
  
return pair;  


# 제출한 코드
        int pair = 0;
        Collections.sort(ar);
        for (int i=0; i<ar.size()-1; i++) {
            if (ar.get(i) == ar.get(i+1)) {
                pair++;
                i++;
            }
        }
        return pair;
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
     * Complete the 'sockMerchant' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER n
     *  2. INTEGER_ARRAY ar
     */

    public static int sockMerchant(int n, List<Integer> ar) {
    // Write your code here
        int pair = 0;
        Collections.sort(ar);
        for (int i=0; i<ar.size()-1; i++) {
            if (ar.get(i) == ar.get(i+1)) {
                pair++;
                i++;
            }
        }
        return pair;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> ar = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.sockMerchant(n, ar);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

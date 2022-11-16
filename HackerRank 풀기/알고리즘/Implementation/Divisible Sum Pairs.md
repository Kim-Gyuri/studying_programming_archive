## 문제
array가 주어졌을 때,  arr[i] + arr[j]가 k으로 나누어지는 경우의 수를 출력한다.

### Sample
```
# Sample Input
6 3            
1 3 2 6 1 2 


# Sample Output
 5
 
 
 

# Explanation 
n = 6, k = 3
ar = [1, 3, 2, 6, 1, 2]을 입력으로 받았을 때,

ar[0] + ar[2] = 1 + 2 = 3
ar[0] + ar[5] = 1 + 2 = 3 
ar[1] + ar[3] = 3 + 6 = 9
ar[2] + ar[4] = 2 + 1 = 3
ar[4] + ar[5] = 1+ 2 = 3

경우의 수가 5가지 이므로 5를 출력한다.
```

## 풀이
+  ar[i] + ar[j] 값이 k로 나누어지는 경우를 찾기 위해, k로 나누었을 때 나머지가 0인 경우를 판단하면 된다.
+  for()문을 돌면서 if(나머지=0)인 경우를 카운팅한다.

```
# 코드 스케치
포인트: ar[]를 모두 돌면서 판단한다.
for (ar를 도는데 -> ar[i])
 for (ar를 돌면서 ar[j]는 i+1으로 다음 index에서 찾아본다.) 
    if (나머지 == 0 인 경우) 카운팅을 한다. ++
    
    
    
    
# 작성한 코드    
        int result = 0;
        for (int i=0; i<ar.size(); i++) {
            for(int j=i+1; j<ar.size(); j++) {
                if ((ar.get(i)+ar.get(j))%k == 0) result += 1;
            }
        }
        return result;
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
     * Complete the 'divisibleSumPairs' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER n
     *  2. INTEGER k
     *  3. INTEGER_ARRAY ar
     */
    public static int divisibleSumPairs(int n, int k, List<Integer> ar) {
    // Write your code here
        int result = 0;
        for (int i=0; i<ar.size(); i++) {
            for(int j=i+1; j<ar.size(); j++) {
                if ((ar.get(i)+ar.get(j))%k == 0) result += 1;
            }
        }
        return result;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> ar = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.divisibleSumPairs(n, k, ar);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

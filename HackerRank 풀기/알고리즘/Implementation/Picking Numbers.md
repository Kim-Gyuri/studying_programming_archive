## 문제
+ 기준을 충족하는 가장 긴 sub Array 길이를 출력한다.
+ 원소들끼리는 절대적인 차이가 1 이하이다.

### Sample
```
# Sample Input 0
6
4 6 5 3 3 1


# Sample Output 0
3


# Explanation
[433]이 가장 길다. 
subArray.length = 3
```

## 풀이
```
# 코드 스케치
i=0; j=0;            #i는 왼쪽 원소 index, j는 오른쪽 원소 index
Collections.sort(a); #오름차순으로 정렬한다.

while(오른쪽 원소가 마지막 원소가 될 때가지) {
  left = a.get(i);
  right = a.get(j); #처음 원소부터 2개씩 왼/오 원소를 가지고,
  if (두 원소 값차이가 1이하인 경우) 원소길이 = Math.max(0, (j-i)+1);  #j-i는 값이 고정으로 1이다, 조건에 만족했을 때 길이가 1씩 증가함
  
  
  
  

# 작성한 코드
        int i=0; int j=1;
        int chosen = 0;
        Collections.sort(a);
        
        while(j<a.size()) {
            int left = a.get(i);
            int right = a.get(j);
            if (Math.abs(left - right) <= 1) {
                chosen = Math.max(chosen, (j-i)+1);
                ++j;
                continue;
            }
            ++i;
        }
        return chosen;
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
     * Complete the 'pickingNumbers' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER_ARRAY a as parameter.
     */

    public static int pickingNumbers(List<Integer> a) {
    // Write your code here
        int i=0; int j=1;
        int chosen = 0;
        Collections.sort(a);
        
        while(j<a.size()) {
            int left = a.get(i);
            int right = a.get(j);
            if (Math.abs(left - right) <= 1) {
                chosen = Math.max(chosen, (j-i)+1);
                ++j;
                continue;
            }
            ++i;
        }
        return chosen;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> a = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.pickingNumbers(a);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

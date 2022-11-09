## 문제
+ 숫자를 입력받아 List.get(i)을 차례로 1개씩 뺀 총합을 구한다.
+ 총합 중에서 가장 작은 값과 가장 큰 값을 각각 출력한다.
+ 단 총합이 32 bit Integer보다 클 수 있다.

## 풀이
```
(1) 총합을 저장할 변수는 큰 타입 long으로 선언한다.
long total = 0;


(2) 일단 List<Integer> 원소 총합을 구한다.
for (Integer i : arr) total += i;


(3) 최소값,최대값은 Math.max() Math.min()을 사용해서 구한다.
total-list.get(i)를 하나씩 뺀 값을 비교해서 구한다.

long min = total; long max = 0;
for (Integer i : arr) {
  max = Math.max(max, total-i);
  min = Math.min(min, total-i);
}
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
     * Complete the 'miniMaxSum' function below.
     *
     * The function accepts INTEGER_ARRAY arr as parameter.
     */

    public static void miniMaxSum(List<Integer> arr) {
    // Write your code here
        long total = 0;
        for (Integer i : arr) total += i;
        
        long min = total; long max = 0;
        for (Integer i : arr) {
            max = Math.max(max, total-i);
            min = Math.min(min, total-i);
        }
        System.out.println(min +" " + max);
       

}
    }
public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        List<Integer> arr = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        Result.miniMaxSum(arr);

        bufferedReader.close();
    }
}
```

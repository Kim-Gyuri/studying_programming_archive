## 문제
+ 각 분수의 소수점 값을 소수점 뒤에 6자리씩 출력한다. 
+ arr 원소 중에서 순서대로 양수,음수,0의 개수를  각각 arr_size로 나눠 몫을 구한다.

### 예
```
# Sample Input
6               
-4 3 -9 0 4 1   


# Sample Output
0.500000
0.333333
0.166667



# Explanation
arr[] size n = 6
arr = [-4, 3, -9, 0, 4, 1]
양수인 3, 4, 1를 각각 arr_size로 나눠 몫 소수점 6자리까지 구한다.
```

## 풀이
```
(1) 양수,음수,0을 구분하기
if (양수)
else if (음수)
else (0인 경우)
if문으로 나눠 해당되면 각각 카운팅 해준다.


(2) 소수점 6자리
System.out.printf("%.6f%n",  p/arr.size());
%.6f%n으로 소수점 6자리까지 표현하고 줄바꿈해준다.
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
     * Complete the 'plusMinus' function below.
     *
     * The function accepts INTEGER_ARRAY arr as parameter.
     */

    public static void plusMinus(List<Integer> arr) {
    // Write your code here
    float p=0; float n =0; float z=0;
    for (Integer number : arr) {
        if (number>0) p++;
        else if (number<0) n++;
        else z++; 
    }
    System.out.printf("%.6f%n",  p/arr.size());
    System.out.printf("%.6f%n", n/arr.size());
    System.out.printf("%.6f%n", z/arr.size());
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> arr = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        Result.plusMinus(arr);

        bufferedReader.close();
    }
}
```

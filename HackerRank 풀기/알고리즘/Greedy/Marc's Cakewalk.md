## 문제
만약 마크가 지금까지 컵케이크를 n개 먹었다면, <br> 
칼로리가 있는 컵케이크를 먹은 후에 그는 몸무게를 유지하기 위해 적어도 2ⁿ x C(miles)을 걸어야 한다.

### Constraints
+ 1 ≤ n ≤ 40
+ 1 ≤ c[i] ≤ 1000

### Sample
```
# Input 0
3
1 3 2


# Output 0
11
```
 
> Eat the cupcake with c1=3 calories, so miles = 0 + (3 x 2^0) = 3 <br>
> Eat the cupcake with c2=2 calories, so miles = 3 + (3 x 2^1) = 7 <br>
> Eat the cupcake with c3=1 calories, so miles = 7 + (3 x 2^2) = 11 <br>

## 풀이
c[i]를 내림차순으로 정렬하고, c[i] x 2ⁿ를 해주면서 더해주면 된다.


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
     * Complete the 'marcsCakewalk' function below.
     *
     * The function is expected to return a LONG_INTEGER.
     * The function accepts INTEGER_ARRAY calorie as parameter.
     */
    public static long marcsCakewalk(List<Integer> calorie) {
    // Write your code here
    Collections.sort(calorie, Collections.reverseOrder());
    long sum = 0;
    for (int i=0; i<calorie.size(); i++) {
        sum += Math.pow(2,i) * calorie.get(i);
    }
    return sum;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> calorie = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        long result = Result.marcsCakewalk(calorie);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```


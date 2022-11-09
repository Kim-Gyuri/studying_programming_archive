## 문제
3개씩 갖는 리스트 2개를 비교한다. <br>
a[i] vs b[i] 원소 큰 값을 비교하여, 각각의 score를 출력한다.

### 풀이
+ 간단하게 if문으로 (a.get(i)> b.get(i)) 비교해준다.
+ a_Score, b_Score 각각 카운팅했다가 출력해준다.


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
     * Complete the 'compareTriplets' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY a
     *  2. INTEGER_ARRAY b
     */

    public static List<Integer> compareTriplets(List<Integer> a, List<Integer> b) {
        List<Integer> results = new ArrayList<>();
        int aNum = 0; 
        int bNum=0;
        for (int i=0; i<a.size(); i++) {
            if (a.get(i)> b.get(i)) aNum++;
            else if (b.get(i)>a.get(i)) bNum++;
        }
        results.add(aNum);
        results.add(bNum);
        return results;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        List<Integer> a = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> b = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.compareTriplets(a, b);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining(" "))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

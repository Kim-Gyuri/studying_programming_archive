## 문제
scores Array를 입력받아 Highest, Lowest score가 갱신되는 횟수를 출력한다.

### Sample
```
# Sample Input 1
10
3 4 21 36 10 28 35 5 24 42


# Sample Output 1
4 0
```

> `Explanation` <br>
> index 0~9까지 총 10개 score를 입력받는다. <br>
> index 순서대로 score, Highest, Lowest를 작성한 표를 보면 알 수 있다. <br>
> Highest : 3->4, 4->21, 21->36, 36->42 <br>
> Lowest : index0이 가장 작으므로 변화가 없다.

|0|1|2|3|4|5|6|7|8|9|
|---|---|---|---|---|---|---|---|---|---|
|3|4|21|36|10|28|35|5|24|42|
|3|4|21|36|36|36|36|36|36|42|
|3|3|3|3|3|3|3|3|3|3|


## 풀이
+ Highest, Lowest의 초기값은 scores Array의 index 0인 값이다.
+ scores Array를 돌면서 해당 초기값보다 크면 Highest 값을 바꿔주면서 카운팅(h++)한다.
+ scores Array를 돌면서 해당 초기값보다 작으면 Lowest 값을 바꿔주면서 카운팅(l++)한다.
+ h,l을 담은 Array를 반환한다.

```
# 코드 스케치
for () {
  if (큰값) h++
  else (작은값) l++ 
}
return Arrays.asList(h,l);



# 제출한 코드
        int Highest = scores.get(0);
        int Lowest = scores.get(0);
        int h = 0;
        int l = 0;
        for (int i=0; i<scores.size(); i++) {
            if (scores.get(i)> Highest) {
                Highest = scores.get(i);
                h++;
            } else if (scores.get(i)<Lowest) {
                Lowest = scores.get(i);
                l++;
            }
        }
        return Arrays.asList(h,l);
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
     * Complete the 'breakingRecords' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts INTEGER_ARRAY scores as parameter.
     */

    public static List<Integer> breakingRecords(List<Integer> scores) {
    // Write your code here
        int Highest = scores.get(0);
        int Lowest = scores.get(0);
        int h = 0;
        int l = 0;
        for (int i=0; i<scores.size(); i++) {
            if (scores.get(i)> Highest) {
                Highest = scores.get(i);
                h++;
            } else if (scores.get(i)<Lowest) {
                Lowest = scores.get(i);
                l++;
            }
        }
        return Arrays.asList(h,l);
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> scores = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.breakingRecords(scores);

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

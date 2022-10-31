## 문제
+ contests에서 각 대회가 중요하면 1이고 중요하지 않으면 0이다.
+ k는 중요한 대회에서 질 수 있는 횟수이다.
+ contests에서 지면 행운이 높아지고 이기면 행운이 낮아진다.
+ 최대 행운이 얼마인지 구하는 문제이다.

### 포인트
리턴값 int ans = 0이라고 하면, <br>
+ 중요하지 않는 대회 행운값은 먼저 더해준다.
+ 중요한 대회 경우, k범위에 따른 판단을 구현해야 한다.
+ 중요한 대회 횟수 = K 경우, ans에 중요한 대회의 행운값을 더해 결과를 리턴한다.
+ 중요한 대회 횟수 < k 경우 (=값을 d로 가정하),  ans에 중요한 대회의 d만큼 작은 행운은 빼고, 나머지는 더해 결과를 리턴한다.

### 풀이
```
    public static int luckBalance(int k, List<List<Integer>> contests) {
    // Write your code here
        int totalLuckBalance = 0;
        List<Integer> important = new ArrayList<Integer>();
        for (List<Integer> contest : contests) {
            if (contest.get(1)==1) important.add(contest.get(0));
            else totalLuckBalance += contest.get(0);
        }
        Collections.sort(important, Collections.reverseOrder());
        for (int i=0; i<important.size(); i++) {
            totalLuckBalance += (i<k?1:-1) * (important.get(i));
        }
        return totalLuckBalance;
    }
    
#1 중요한 대회는 따로 List를 만들어 저장해둔다.
for ( List<Integer> contest ... ) 에서  t값이 1인 경우 
중요한 대회이므로 List.add()해둔다.
-> contests 2차원 리스트는 contest들을 index 2크기 만큼(행운값, 중요도 -> 2칸)만 담으므로
if (중요도가 1인 경우) important.add(해당 대회 행운값);
else (중요도가 0인 경우) 각 대회의 행운값을 더한다. 



#2 중요한 대회 정보를 담은 List<>를 내림차순으로 정렬한다,
for (k-중요대회횟수 차이만큼=d) {...
차이만큼 작은 행운값은 축척된 행운값에서 빼고, 나머지는 더한다.  (삼항 연산자로 d만큼은 빼고, 나머지는 더해준다)
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
     * Complete the 'luckBalance' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER k
     *  2. 2D_INTEGER_ARRAY contests
     */
    public static int luckBalance(int k, List<List<Integer>> contests) {
    // Write your code here
        int totalLuckBalance = 0;
        List<Integer> important = new ArrayList<Integer>();
        for (List<Integer> contest : contests) {
            if (contest.get(1)==1) important.add(contest.get(0));
            else totalLuckBalance += contest.get(0);
        }
        Collections.sort(important, Collections.reverseOrder());
        for (int i=0; i<important.size(); i++) {
            totalLuckBalance += (i<k?1:-1) * (important.get(i));
        }
        return totalLuckBalance;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        List<List<Integer>> contests = new ArrayList<>();

        IntStream.range(0, n).forEach(i -> {
            try {
                contests.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        int result = Result.luckBalance(k, contests);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

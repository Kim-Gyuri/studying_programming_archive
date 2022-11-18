## 문제
+ 최소 n명이 제 시간에 없다면 수업이 취소되므로 YES, n명이 있다면 NO를 출력한다.
+ input 1 Line :  the number of test cases.
+ input 2 Line :  n, k. (the number of students, The professor wants at least k students) 


## 풀이
```
# 코드 스케치
arrived = 0;    #도착한 학생 수를 저장할 변수 선언
for (n 명만큼 돌면서) {
    if (일찍 왔다면) arrived++; #도착한 학생 +1
}
k > arrived  -> 수업 취소됨 YES  #여기 판단을 삼항연산자로 구현한다.
k < arrived  -> 수업 진행됨 NO



# 제출한 코드
        int arrived = 0;
        for (Integer i : a) {
            if (i<=0) arrived++;
        }
        return k > arrived ? "YES" : "NO"; 
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
     * Complete the 'angryProfessor' function below.
     *
     * The function is expected to return a STRING.
     * The function accepts following parameters:
     *  1. INTEGER k
     *  2. INTEGER_ARRAY a
     */

    public static String angryProfessor(int k, List<Integer> a) {
    // Write your code here
        int arrived = 0;
        for (Integer i : a) {
            if (i<=0) arrived++;
        }
        return k > arrived ? "YES" : "NO"; 
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, t).forEach(tItr -> {
            try {
                String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

                int n = Integer.parseInt(firstMultipleInput[0]);

                int k = Integer.parseInt(firstMultipleInput[1]);

                List<Integer> a = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                    .map(Integer::parseInt)
                    .collect(toList());

                String result = Result.angryProfessor(k, a);

                bufferedWriter.write(result);
                bufferedWriter.newLine();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

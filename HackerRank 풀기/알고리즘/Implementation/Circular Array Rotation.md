## 문제
입력받은 배열을 회전했을 때 결과를 q[i]로 출력한다.
+ input 1 Line : n k q
> n : array size <br>
> k  : a number of right circular rotations <br>
> q :  number of queries

<br>

+ input 2 Line : a[i] 입력한 배열의 요소들
+ input 3 Line ~ : 회전했을 때, 해당 배열의 index
+ output : 회전을 끝낸 배열 a의 값 a.get(index)를 List로 반환받고 출력한다.

<br><br>

### Sample
```
# Sample Input 0
3 2 3
1 2 3
0
1
2


# Sample Output 0
2
3
1



# Explanation
a = [1,2,3]을 오른쪽으로 회전 2번 한다면,
r1 -> 3 1 2
r2 -> 2 3 1

2번 회전 했을 때 [2,3,1]이 된다.
index로 읽으면 아래와 같다.
a[0] = 2
a[1] = 3
a[2] = 1
```

## 풀이
```
#shift rotate -> 회전은 Collections.rotate() 메소드를 이용한다.
Collections.rotate(list,k) 
메소드를 호출한다면 맨뒤의 요소를 하나씩 k개를 꺼내서
맨앞 요소자리에 넣고 다른 요소들은 뒤로 한칸씩밀리게 되는 것이다.

#포인트
queries의 요소들이 index가 된다. -> queries.get(i) = index
a.get(index)
for (실행할 queries개수만큼 반복)  { queries.set(순서대로, a.get(index)); }



# 코드 스케치
List<Integer> circularArrayRotation(List<Integer> a, int k, List<Integer> queries) {
# queries = 회전을 끝낸 후, 값을 담을 공간
Collections.rotate(a, k);
for (q쿼리를 볼때) {
  queries.set(i, a.get(queries.get(i))); 
}
return queries;
  



# 제출한 코드
        Collections.rotate(a, k);
        for (int i=0; i<queries.size(); i++) {
            queries.set(i, a.get(queries.get(i)));
        }
        return queries;
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
     * Complete the 'circularArrayRotation' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY a
     *  2. INTEGER k
     *  3. INTEGER_ARRAY queries
     */

    public static List<Integer> circularArrayRotation(List<Integer> a, int k, List<Integer> queries) {
    // Write your code here
        Collections.rotate(a, k);
        for (int i=0; i<queries.size(); i++) {
            queries.set(i, a.get(queries.get(i)));
        }
        return queries;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        int q = Integer.parseInt(firstMultipleInput[2]);

        List<Integer> a = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> queries = IntStream.range(0, q).mapToObj(i -> {
            try {
                return bufferedReader.readLine().replaceAll("\\s+$", "");
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .map(String::trim)
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.circularArrayRotation(a, k, queries);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

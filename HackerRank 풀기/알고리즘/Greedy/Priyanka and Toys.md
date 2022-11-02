## 문제
The shipping company has a requirement that all items loaded in a container  <br>
must weigh less than or equal to 4 units plus the weight of the minimum weight item. <br>
> 대충 최소무게 + (4단위)로 범위제한을 맞춰달라는 것 같다.

<br>

Return the integer value of the number of containers Priyanka must contract to ship all of the toys.
> 최소 필요한 컨테이너 수를 출력한다.


### 예시
```
# Sample Input
8
1 2 3 21 7 12 14 21


# Sample Output
4
```

<br>

### 예시 -설명
```
총 4개로 나누어진다.
unit[0] = 1 2 3 (1~5)
unit[1] = 7     (7~11)
unit[2] = 12 14 (12~16)
unit[3] = 21 21 (21~25)
```

## 풀이
+ 간단한 문제로, List 정렬 후, 4개로 끊어주면 될 것 같다.

```
        int units = 0, container = -1;  #초기값 설정: units =0;, container는 0보다 작게 -1로 둔다.
        Collections.sort(w); #일단 오름차순으로 정렬한다.
        for (int weight : w) { 
            if (weight > container) {  # 초기값 + 4 단위로 되도록, 
                units++;
                container = weight + 4;   #container단위를 +4를 더해, 반복작업을 해준다.
            }
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
     * Complete the 'toys' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER_ARRAY w as parameter.
     */

    public static int toys(List<Integer> w) {
    // Write your code here
        int units = 0, container = -1;
        Collections.sort(w);
        for (int weight : w) {
            if (weight > container) {
                units++;
                container = weight + 4;
            }
        }
        return units;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> w = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.toys(w);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
        

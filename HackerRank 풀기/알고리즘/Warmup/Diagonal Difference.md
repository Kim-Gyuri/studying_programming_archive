## 문제
정사각형으로 된 리스트를 받아, 2개의 대각선의 차이를 출력한다.

### 예시
```
For example, the square matrix  is shown below:  ---> return 2
3
1 2 3
4 5 6
9 8 9  

matrix : 3*3
left-to-right : 1 5 9
right-to-left : 3 5 9

return = | (1+5+9) - (3+5+9) |
```

## 풀이
```
# 정사각형 리스트 
예시: n=3,
matrix[3*3]인 사각형이 된다,

[0,0] [0,1] [0,2]

[1,0] [1,1] [1,1]

[2,0] [2,1] [2,2]

(1) leftToRight += arr.get(i).get(i);
[0,0] [1,1] [2,2]

(2) RightToLeft += arr.get(i).get(arr.size()-1-i);
[0,2] [1,1] [2,0]



# 대각선 차이값 계산
return Math.abs(leftToRight-RightToLeft);
Math.abs()로 절대값을 리턴해준다.
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
     * Complete the 'diagonalDifference' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts 2D_INTEGER_ARRAY arr as parameter.
     */
   
    public static int diagonalDifference(List<List<Integer>> arr) {
    // Write your code here
    int leftToRight = 0; 
    int RightToLeft = 0;
    for (int i=0; i<arr.size(); i++) {
        leftToRight += arr.get(i).get(i);
        RightToLeft += arr.get(i).get(arr.size()-1-i);
    }
    return Math.abs(leftToRight-RightToLeft);

    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<List<Integer>> arr = new ArrayList<>();

        IntStream.range(0, n).forEach(i -> {
            try {
                arr.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        int result = Result.diagonalDifference(arr);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

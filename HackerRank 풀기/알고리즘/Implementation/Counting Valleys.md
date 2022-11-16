## 문제
+ U(Up)와 D(Down)로 이루어진 문자를 받고, 계곡이 몇개 있는지 찾는 문제이다.
+ sea level에서 시작해서 아래로 내려갔다가 다시 sea level로 올라오면 1개의 계곡이 된다.

### Sample
```
# Sample Input
8
UDDDUDUU


# Sample Output
1




# Explanation
If we represent _ as sea level, a step up as /, and a step down as \, the hike can be drawn as:

_/\      _
   \    /
    \/\/
    
    
u(4) = d(4)가 될 때 1개의 계곡이 된다.    
```


## 풀이
```
# 코드 스케치
steps = 총 u/d 개수
path = u와 d로 이루어진 문자열

char[] path_arr = path.toCharArray(); #먼저 문자열을 배열로 변환한다.
int start = 0; #u를 +, d를 -로 했을 때 계산할 변수 선언
int count = 0; #계곡 수 변수 선언
for (steps 개수만큼 돌면서) {
  if (path_arr[i]가 D인 경우) start - 1를 해준다.
  if (path_arr[i]가 U인 경우) start + 1, ---> if (start가 0이 되는 순간) 계곡이 1개 생성된다.



# 제출한 코드
        char[] path_arr = path.toCharArray();
        int start = 0; int count = 0;
        
        for (int i=0; i<steps; i++) {
            if (path_arr[i] == 'D') start--;
            if (path_arr[i] == 'U') {
                start++;
                if (start == 0) count++;
            }
        }
        return count;
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
     * Complete the 'countingValleys' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER steps
     *  2. STRING path
     */

    public static int countingValleys(int steps, String path) {
    // Write your code here
        char[] path_arr = path.toCharArray();
        int start = 0; int count = 0;
        
        for (int i=0; i<steps; i++) {
            if (path_arr[i] == 'D') start--;
            if (path_arr[i] == 'U') {
                start++;
                if (start == 0) count++;
            }
        }
        return count;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int steps = Integer.parseInt(bufferedReader.readLine().trim());

        String path = bufferedReader.readLine();

        int result = Result.countingValleys(steps, path);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```
    

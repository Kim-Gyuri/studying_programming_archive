## 문제
주어진 그리드의 열이 사전순 정렬이면 YES를 출력한다.


#### 예시 
`Sample Input` <br>
```
STDIN   Function
-----   --------
1       t = 1
5       n = 5
ebacd   grid = ['ebacd', 'fghij', 'olmkn', 'trpqs', 'xywuv']
fghij
olmkn
trpqs
xywuv
```


`Sample Output` <br>
```
YES
```

<br>

> `Explanation`  <br>
>  grid[] -> 먼저 grid[i]를 알파벳순 정렬을 해준다. <br>
> grid[i] < grid[i+1] 순으로 정렬되는지 확인 -> 맞다면 YES <br>
>  <br>
abcde <br>
fghij <br>
klmno <br>
pqrst <br>
uvwxy <br>


## 풀이
```
`1` String을 char로 전환한다.
  for() : char[] element = grid.get(i).toCharArray();
  
  # 자바 toCharArray():
  메소드는 문자열을 char형 배열로 바꿔준다. 
  반환되는 배열의 길이는 문자열의 길이와 같다.
  주의 문자열의 공백 또한 인덱스에 포함한다.
  
  
`2` grid[i] 각각 사전순 정렬하기
Arrays.sort(element_ch);


`3` grid[] -> 요소를 정렬한 값으로 교체하기
grid.add(i, Arrays.toString(element_ch));
grid.remove(i+1);


`4`  사전순 정렬이 맞는지 판단하기
for()루프를 돌려, 리스트 안을 탐색하면서 if(현재값>다음값)에 해당되면 "NO"로 판단,
for()루프를 다 돌고도 if()를 통과했다면 "YES"로 판단한다.
for() {
다음열값 < 현재값 이 맞다면 return "NO"
}
return "YES"

-> if()비교
다음열값 맨 앞 문자 - 현재값 맨 앞 문자를 비교한다.
if(grid.get(j+1).charAt(i)<grid.get(j).charAt(i))




`제출코드` 
    // Write your code here
        for (int i=0; i<grid.size(); i++) {
            char[] element_ch = grid.get(i).toCharArray();
            Arrays.sort(element_ch);
            grid.add(i, Arrays.toString(element_ch));
            grid.remove(i+1);
        }
        
        for (int i=0; i<grid.get(0).length()-1; i++) {
            for (int j=0; j<grid.size()-1; j++) {
                 if(grid.get(j+1).charAt(i)<grid.get(j).charAt(i)) return "NO";
            }
        }
        return "YES"; 
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
import java.util.Collections;

class Result {

    /*
     * Complete the 'gridChallenge' function below.
     *
     * The function is expected to return a STRING.
     * The function accepts STRING_ARRAY grid as parameter.
     */
    public static String gridChallenge(List<String> grid) {
    // Write your code here
        for (int i=0; i<grid.size(); i++) {
            char[] element_ch = grid.get(i).toCharArray();
            Arrays.sort(element_ch);
            grid.add(i, Arrays.toString(element_ch));
            grid.remove(i+1);
        }
        
        for (int i=0; i<grid.get(0).length()-1; i++) {
            for (int j=0; j<grid.size()-1; j++) {
                 if(grid.get(j+1).charAt(i)<grid.get(j).charAt(i)) return "NO";
            }
        }
        return "YES"; 
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int t = Integer.parseInt(bufferedReader.readLine().trim());

        IntStream.range(0, t).forEach(tItr -> {
            try {
                int n = Integer.parseInt(bufferedReader.readLine().trim());

                List<String> grid = IntStream.range(0, n).mapToObj(i -> {
                    try {
                        return bufferedReader.readLine();
                    } catch (IOException ex) {
                        throw new RuntimeException(ex);
                    }
                })
                    .collect(toList());

                String result = Result.gridChallenge(grid);

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

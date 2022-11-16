## 문제
+ Array를 입력받아 해당 조건에 맞는 subArray 경우의 수를 출력한다.
+ 입력 1 Line : 입력 Array size
+ 입력 2 Line : 입력 Array elements
+ 입력 3 Line : m과 n. (m: 합의 크기, n:subArray에 들어갈 elements 개수) 
+ 출력 : n개의 원소를 더했을 때 합의 크기가 m이 되는 경우를 카운팅하라.

### Sample
```
# Sample Input 0
5
1 2 1 3 2
3 2


# Sample Output 0
2




# Explanation
arr = [1,2,1,3,2]
query = n(=2)개를 더해 m(=3)을 만들어라. 

sub_arr = [1,2]
sub_arr = [2,1]
답은 2가 된다.
```


## 풀이

```
# 코드 스케치
s = 입력 받은 Array
d = query 조건 sum의 값
m = query 조건 subArray의 elements 개수

index i ~ i+m 까지 끊어서 sub_Arr를 만든다.
해당 sub_Arr의 sum은 stream().mapToInt().sum()을 이용하여 구한다.

for ( s List<>를 m개만큼 짤라 돌면서) 
  n = s.subList(i, m+i)....sum() #subArray sum을 구한다.
  if (n == d)  subArr.add(n) # 조건 sum값과 같다면, 경우 수로 카운팅한다.
    # 예: arr = [1,2,1,3,2] -> subArr = [3,3]  -> subArr.size() = 2
    
    
    
    
# 작성한 코드    
       List<Integer> subArr = new ArrayList<>();
       for (int i=0; i<=s.size()-m; i++) {
           int n = s.subList(i, m+i).stream().mapToInt(Integer::intValue).sum();
           if (n == d) subArr.add(n);
       }
       return subArr.size();
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
     * Complete the 'birthday' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY s
     *  2. INTEGER d
     *  3. INTEGER m
     */
  
    public static int birthday(List<Integer> s, int d, int m) {
    // Write your code here
       List<Integer> subArr = new ArrayList<>();
       for (int i=0; i<=s.size()-m; i++) {
           int n = s.subList(i, m+i).stream().mapToInt(Integer::intValue).sum();
           if (n == d) subArr.add(n);
       }
       return subArr.size();

    }
    

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> s = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int d = Integer.parseInt(firstMultipleInput[0]);

        int m = Integer.parseInt(firstMultipleInput[1]);

        int result = Result.birthday(s, d, m);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

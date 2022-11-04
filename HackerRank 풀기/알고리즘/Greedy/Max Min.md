## 문제
주어진 배열에서, unfairness is minimized인 원소를 골라 배열을 만들어야 한다. <br>
`Unfairness of an array = max(arr) = min(arr')`
### 입력
+ 1줄 : 주어진 배열의 크기 n 
+ 2줄 : unfairness 각각 배열의 크기 k 
+ 3줄 : 주어진 배열의 원소들 arr[i]

### 출력
+ int: the minimum possible unfairness

#### Constraints
> 2 ≤ n ≤ 10⁵ <br>
> 2 ≤ k ≤ n <br>
> 0 ≤ arr[i] ≤ 10⁹ (최솟값은 0) <br>

### 예시
+ sample 0
```
# Sample Input 0
7
3
10
100
300
200
1000
20
30


# Sample Output 0
20


# Explanation 0
selecting the 3 integers[: 10,20,30],
unfairness equals 20, --->max(10,20,30) - min(10,20,30) = 30 - 10 = 20
```

<br>

+ sample 1
```
# Sample Input 1
5
2
1
2
1
2
1


# Sample Output 1
0


# Explanation 1
selecting the 2 integers[2,2] or integer[1,1],
그래서 2-2 or 1-1 이므로 0이 된다.
```

## 풀이 스케치
+ loop length : len(arr)-k+1
```
K는 불공정성을 시험할 부분 배열의 길이이다.
-> 주어진 배열을  k 크기만큼 잘라서 Unfairness을 찾아야 한다.


# 예를들면 
:주어진 배열의 길이가 5이고 k가 2라면,
(1) 길이 2의 모든 하위 배열을 테스트해야 한다.
(2)for (int i=0; i<arr.size()-k+1; i++) {
    --> 정렬된 배열을 가지고 k-arr.size() 만큼 이동시키면서 
        모든 값을 기록한후에 마지막에 min_unfairness(최솟값)을 출력해주면 된다.
```

<br>

+  current_unfairness 구하기
```
(1) 해당 최솟값을 찾기 위해 변수 하나를 선언해준다.
    int min_unfairness = Integer.MAX_VALUE; (-> 이 값은 대충 a[i]와 비교하기 위해 큰 값을 줌)
    
(2) int current_unfairness = arr.get(i+(k-1)) - arr.get(i); 
    현재 최솟 unfairness값을 구한다.
    예: arr[1,2,4,7] k=2일 때, arr`=[4,7] unfairness = max(4,7) min(4,7) = 7-4 = 3 
    i=2일 때,  "arr[3]-arr[2]= 7-4 = 3 
    
(3) 반복문을 돌려서 min_unfairness보다 작으면 current_unfairness을 바꿔주면서 최솟갓을 구한다. 
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
     * Complete the 'maxMin' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER k
     *  2. INTEGER_ARRAY arr
     */

    public static int maxMin(int k, List<Integer> arr) {
    // Write your code here
        Collections.sort(arr);
        int min_unfairness = Integer.MAX_VALUE;
        for (int i=0; i<arr.size()-k+1; i++) {
            int current_unfairness = arr.get(i+(k-1)) - arr.get(i);
            if (current_unfairness < min_unfairness) {
                min_unfairness = current_unfairness;
            }
        }
        return min_unfairness;
        
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        int k = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> arr = IntStream.range(0, n).mapToObj(i -> {
            try {
                return bufferedReader.readLine().replaceAll("\\s+$", "");
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .map(String::trim)
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.maxMin(k, arr);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

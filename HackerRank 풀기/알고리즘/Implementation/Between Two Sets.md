## 문제
```
# Sample Input
2 3
2 4
16 32 96

# Sample Output
3

# Explanation
2와 4는 4,8,12,16으로 균등하게 나뉩니다.
4, 8, 16은 16, 32, 96으로 균등하게 나뉩니다.
4, 8, 16은 a의 각 원소가 인자이고 b의 모든 원소의 인자인 유일한 세 숫자이다.
```

## 풀이
+ a배열의 공배수와 b배열의 공약수의 교집합을 구하면 된다.

```
# 위의 예시는 
a[2,4] b[16,32,96]일 때
a[] 공배수 : 4, 8, 12, 16, 20, 24, 28, 32 ...96
b[] 공약수 : 1, 2, 4, 8, 16
-> 둘의 교집합은 4,8,16이다.

a[]과 b[]의 사이 최소값과 최대값 범위값 사이의 교집합 원소를 구하면 되는데,

# a[] 공배수
범위값 전체를 돌면서 a[]의 공배수값이 있는지 판단하면 된다.
해당 범위값 % a[i]를 했을 때 나머지가 없으면 true로 판단하자.

# b[] 공약수
범위값 전체를 돌면서 b[]값이 공약수인지 판단하면 된다.
b[i] % 해당 범위값을 했을 때 나머지가 없다면 true로 판단하자.




# 해당 제출코드 부분
    public static int getTotalX(List<Integer> a, List<Integer> b) {
    // Write your code here
        int min = Collections.min(a);  #최소값
        int max = Collections.max(b);  #최대값
        int count = 0;
        for (int i=min; i<=max; i++) {  # 교집합인 원소를 카운팅하여 반환한다.
            if (AreAllElementsOfArrayAFactorOf(a, i) && IsIntegerAFactorOfAllElementsOfArray(b, i)) count++;
        }
        return count;
    }
    
    # a List<>의 공배수를 구하는 메소드
    private static boolean AreAllElementsOfArrayAFactorOf(List<Integer> arr, int i) {
        for (Integer a : arr)
            if (i%a != 0) return false;
        return true;
        
    }

  
  # b List<>의 공약수를 구하는 메소드
    private static boolean IsIntegerAFactorOfAllElementsOfArray(List<Integer> arr, int i) {
        for (Integer a : arr)
            if (a%i != 0) return false;
        return true;
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
     * Complete the 'getTotalX' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY a
     *  2. INTEGER_ARRAY b
     */
    public static int getTotalX(List<Integer> a, List<Integer> b) {
    // Write your code here
        int min = Collections.min(a);
        int max = Collections.max(b);
        int count = 0;
        for (int i=min; i<=max; i++) {
            if (AreAllElementsOfArrayAFactorOf(a, i) && IsIntegerAFactorOfAllElementsOfArray(b, i)) count++;
        }
        return count;
    }
    
    private static boolean AreAllElementsOfArrayAFactorOf(List<Integer> arr, int i) {
        for (Integer a : arr)
            if (i%a != 0) return false;
        return true;
        
    }

    private static boolean IsIntegerAFactorOfAllElementsOfArray(List<Integer> arr, int i) {
        for (Integer a : arr)
            if (a%i != 0) return false;
        return true;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int m = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> arr = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> brr = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int total = Result.getTotalX(arr, brr);

        bufferedWriter.write(String.valueOf(total));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

## 문제
장난감 가격과 지출할 금액의 목록이 주어지면, 그가 살 수 있는 최대 선물 수를 출력한다.

### Sample 
```
# Input
7 50
1 12 5 111 200 1000 10


# Output
4


# Explanation
He can buy only 4 toys at most. 
These toys have the following prices: 1 12 10 5
```

## 풀이
목록을 내림차순 정렬하고, 장난감 1개씩 더해가며 총합을 예산액과 비교하며 카운팅한다.

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
     * Complete the 'maximumToys' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY prices
     *  2. INTEGER k
     */

    public static int maximumToys(List<Integer> prices, int k) {
    // Write your code here
        Collections.sort(prices);
        int courrentCost = 0;
        int numOfToys = 0;
        
        for (int price : prices) {
            if ((courrentCost + price) < k) {
                courrentCost += price;
                numOfToys++;
            } 
        }
        return numOfToys; 
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> prices = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int result = Result.maximumToys(prices, k);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

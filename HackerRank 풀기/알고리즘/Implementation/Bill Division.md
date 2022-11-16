## 문제
It should print Bon Appetit if the bill is fairly split.  <br>
Otherwise, it should print the integer amount of money that Brian owes Anna. <br>

+ n : 주문된 항목의 수
+ k : Anna가 먹지 않는 항목의 index 번호
+ bill[i] : 주문된 음식들
+ b: Anna가 계산서에 기여한 돈의 양

### Sample
```
# Sample Input 1
4 1
3 10 2 9
7


# Sample Output 1
Bon Appetit



# Explanation
bill[] = [3, 10, 2, 9]
k = 1
b = 7
The food that Anna ate : [3, 2, 9]  total = 14
Is it the same split in half? 14/2 == b
그래서 Bon Appetit가 출력된다.



------------------------------------------------
# Sample Input 0
4 1
3 10 2 9
12


# Sample Output 0
5


# Explanation 
bill[] = [3, 10, 2, 9]
k = 1
b = 12
The food that Anna ate : [3, 2, 9]  total = 14
Is it the same split in half? 14/2 != b 
그래서  b - 14/2 값인 5를 출력한다. 
```

## 풀이
+ bill Array를 돌면서 if()조건을 만족한 경우, 해당 값을 모두 더한 cost를 반환한다. 
+ 삼항연산자를 통해 b와 같은 경우는 "Bon Appetit"를 출력한다.

```
# 코드 스케치
for (bill[]를 돌면서) {
  if (index가 k가 아닌 것은) cost=모두 더한 값;
}
출력 : cost/2 == b 이면 "Bon Appetit"를 출력한다.
     : cost2 != b  이면 b - cost/2를 출력한다.


# 제출한 코드
        int cost = 0;
        for (int i=0; i<bill.size(); i++) {
            if (i!=k) cost += bill.get(i);
        }
        System.out.println(cost/2 == b ? "Bon Appetit" : b-cost/2);
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
     * Complete the 'bonAppetit' function below.
     *
     * The function accepts following parameters:
     *  1. INTEGER_ARRAY bill
     *  2. INTEGER k
     *  3. INTEGER b
     */

    public static void bonAppetit(List<Integer> bill, int k, int b) {
    // Write your code here
        int cost = 0;
        for (int i=0; i<bill.size(); i++) {
            if (i!=k) cost += bill.get(i);
        }
        System.out.println(cost/2 == b ? "Bon Appetit" : b-cost/2);
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int n = Integer.parseInt(firstMultipleInput[0]);

        int k = Integer.parseInt(firstMultipleInput[1]);

        List<Integer> bill = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        int b = Integer.parseInt(bufferedReader.readLine().trim());

        Result.bonAppetit(bill, k, b);

        bufferedReader.close();
    }
}
```

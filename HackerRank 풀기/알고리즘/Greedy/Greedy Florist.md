## 문제
꽃가게는 그의 새로운 손님 수와 그가 버는 돈을 최대화하기를 원한다. <br>
이를 위해 그는 각 꽃의 가격을 고객이 이전에 구입한 꽃의 숫자에 더하여 곱하기로 결심합니다. <br> 
첫번째 꽃은 원래 가격이 될 것이고, 다음 꽃은 (0+1) x origin Price이 된다. <br>
그 다음은 (1+1) x origin Price가 된다.

### Sample 
```
# Input 
5 3
1 3 5 7 9


# Output 2
29


# 설명
꽃 종류: 5, 손님: 3
9 (0+1)
7 (0+1)
5 (0+1)
3 (1+1)
1 (1+1)
--> 처음 3종류는 비싼 것을 고른다. ( *곱하기를 해버리면 너무 커지기 때문)
다음 종류부터는 3개씩 (1+1) 이렇게 곱해준다.
```

## 풀이
+ 꽃 리스트를 내림차순 정렬한다.
+ 결제금액 = (index/3 몫 + 1) x 꽃금액 으로 해준다.

```
꽃 종류: 5, 손님: 3  -> k=3
9 (0+1) -> index :0, 0/3=0 <--> (0+0)
7 (0+1) -> index :1   ..
5 (0+1) -> index :2   ..
3 (1+1) -> index :3, 3/3=1 <--> (1+1)
1 (1+1) -> index :4   ..


# 코드
        for (int price : flowers) {
            multiplier = personIndex/k;  # 3개씩 분류한다.
            currentCost += (multiplier+1)*price;  
```            


## 코드
```java
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.regex.*;

public class Solution {
    // Complete the getMinimumCost function below.
    static int getMinimumCost(int k, int[] c) {
        int[] flowers = Arrays.stream(c).boxed().sorted(Collections.reverseOrder()).mapToInt(Integer::intValue).toArray();
        int currentCost = 0;
        int personIndex = 0;
        int multiplier = 0;
        for (int price : flowers) {
            multiplier = personIndex/k;
            currentCost += (multiplier+1)*price;
            personIndex++;
        }
        return currentCost;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] nk = scanner.nextLine().split(" ");

        int n = Integer.parseInt(nk[0]);

        int k = Integer.parseInt(nk[1]);

        int[] c = new int[n];

        String[] cItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

        for (int i = 0; i < n; i++) {
            int cItem = Integer.parseInt(cItems[i]);
            c[i] = cItem;
        }

        int minimumCost = getMinimumCost(k, c);

        bufferedWriter.write(String.valueOf(minimumCost));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```


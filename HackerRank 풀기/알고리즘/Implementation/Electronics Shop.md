## 문제
+ 예산, 구매할 키보드 품번, 구매할 USB Drive 품번을 입력받았을 때 예산에 맞으면 결제금액을 출력한다.
+ 조건을 만족한 경우 해당 cost를 반환하고, 만족하지 못한 경우 -1를 반환한다.
+ (여기서 품번은 1번부터 시작하므로 Array index 0 == 품번 1 이 된다.)

### Sample
```
# Sample Input 1
5 1 1
4
5

# Sample Output 1
-1




# Explanation
입력을 분석해보면 아래와 같다.
예산 :5
구매할 키보드 품번 :1
구매할 USB Drive 품번 :1
키보드 배열 [4]
USB Drive 배열 [5]

예상금액 4+5 = 9
예산을 초과한 금액이므로 -1를 반환해준다.
```

## 풀이
```
# 코드 스케치
int result = -1;  #반환할 값의 초기값은 -1이 된다. (예산을 초과했을 때)
for (키보드 배열을 돌면서) {
  for (USB Drive 배열도 돌리는데) {
       cost(예상금액) = keyboards[i] + drive[j]; 
       if (예상금액은 정수이며, 예산금액보다 작을 때) result = cost; #예산 안의 금액일 때 cost로 갱신해준다.




# 제출한 코드
        int result = -1;
        for (int i=0; i<keyboards.length; i++) {
            for (int j=0; j<drives.length; j++) {
                int cost = keyboards[i] + drives[j];
                if (cost>result && cost<=b) result = cost; 
            }
        } 
        return result;
```

## 코드
```java
import java.io.*;
import java.math.*;
import java.text.*;
import java.util.*;
import java.util.regex.*;

public class Solution {

    /*
     * Complete the getMoneySpent function below.
     */
    static int getMoneySpent(int[] keyboards, int[] drives, int b) {
        /*
         * Write your code here.
         */
        int result = -1;
        for (int i=0; i<keyboards.length; i++) {
            for (int j=0; j<drives.length; j++) {
                int cost = keyboards[i] + drives[j];
                if (cost>result && cost<=b) result = cost; 
            }
        } 
        return result;
    }

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) throws IOException {
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        String[] bnm = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])*");

        int b = Integer.parseInt(bnm[0]);

        int n = Integer.parseInt(bnm[1]);

        int m = Integer.parseInt(bnm[2]);

        int[] keyboards = new int[n];

        String[] keyboardsItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])*");

        for (int keyboardsItr = 0; keyboardsItr < n; keyboardsItr++) {
            int keyboardsItem = Integer.parseInt(keyboardsItems[keyboardsItr]);
            keyboards[keyboardsItr] = keyboardsItem;
        }

        int[] drives = new int[m];

        String[] drivesItems = scanner.nextLine().split(" ");
        scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])*");

        for (int drivesItr = 0; drivesItr < m; drivesItr++) {
            int drivesItem = Integer.parseInt(drivesItems[drivesItr]);
            drives[drivesItr] = drivesItem;
        }

        /*
         * The maximum amount of money she can spend on a keyboard and USB drive, or -1 if she can't purchase both items
         */

        int moneySpent = getMoneySpent(keyboards, drives, b);

        bufferedWriter.write(String.valueOf(moneySpent));
        bufferedWriter.newLine();

        bufferedWriter.close();

        scanner.close();
    }
}
```






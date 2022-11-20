## 문제
+ An integer d is a divisor of an integer n if the remainder of n%d=0.
+ Given an integer, for each digit that makes up the integer determine whether it is a divisor.
+ Count the number of divisors occurring within the integer.
+ (divisors) but 0 is not.
+ input 1 Line : n, the number of test cases.
+ input 2 Line : numbers
+ output : the number of digits in n that are divisors of n

### Sample
```
# Sample Input
2
12
1012


# Sample Output
2
3


# Explanation
12는 1,2로 나누어진다.
12 % 1 = 0
12 % 2 = 0 
12 -> 2가지

1012는 1,0,1,2로 나누어진다.
(0은 제외되고,)
1012 % 1 = 0
1012 % 1 = 0
1012 % 2 = 0
1012 -> 3가지
```


## 풀이
```
# 코드 스케치
init = n;      #초기값
n_divided      #n에 포함된 숫자를 추출
count          #the number of digits in n that are divisors of n 를 저장할 변수

while (n != 0)
    n_divied = n%10;  #n의 자릿수의 숫자를 추출한다, 1의자리부터 추출된다
    n /= 10;          #1의 자리 -> 10자리 -> 이런 식으로 한개씩 추출되도록 한다.
    if (n_divied가 0이 아닐 때까지, 동시에 나머지가 0일 때까지) count++; 
}
return count;




# 제출한 코드
        int init = n; int n_divided = 0; int count = 0; 
        while(n!=0) {
            n_divided = n%10;   
            n/=10;
            if (n_divided!=0 && init%n_divided==0) count++;
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
     * Complete the 'findDigits' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts INTEGER n as parameter.
     */
    public static int findDigits(int n) {
    // Write your code here
        int init = n; int n_divided = 0; int count = 0; 
        while(n!=0) {
            n_divided = n%10;
            n/=10;
            if (n_divided!=0 && init%n_divided==0) count++;
        }
        return count;
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

                int result = Result.findDigits(n);

                bufferedWriter.write(String.valueOf(result));
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

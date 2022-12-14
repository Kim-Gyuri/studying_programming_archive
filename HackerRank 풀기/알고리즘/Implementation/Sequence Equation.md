## 문제
input 1 Line : the number of elements in the sequence. <br>
input 2 Line : elements in the sequence. <br>
output : the values of y for all x in the arithmetic sequence 1 to n. <br>

### Sample
```
# Sample Input 0
3
2 3 1


# Sample Output 0
2
3
1




# Explanation
아래와 같이 값이 주어졌을 때,
p(1) = 2
p(2) = 3
p(3) = 1

p(p(y)) = x를 만족하는 y를 찾는다.

for each 1 to n:
x=1일 때
x = 1 = p(3) = p(p(2)) -> y = 2
x = 2 = p(1) = p(p(3)) -> y = 3
x = 3 = p(2) = p(p(1)) -> y = 1
순서대로 출력하면 된다.
2
3
1
```

## 풀이
```
# 포인트 
indexOf(Object o) : list에 o가 있다면, 해당 index를 반환받는다. (없다면 -1를 반환)


# 코드 스케치
y_values = List<Integer>;                  # y값을 저장할 공간을 선언
for (x가 1~n까지 돌면서) {
  p(n)의 n값, p_y = p.indexOf(x);
  p(p(n))의 n값, y = p.indexOf(p_y+1)+1);  # list의 index는 0부터 이므로 각각 1씩 더해준다.
  y_values.add(y);                         # 리스트에 담아 반환한다.      
  
  
  

# 제출한 코드
        int p_y = 0;
        List<Integer> y_values = new ArrayList<>();
        for (int x=1; x<=p.size(); x++) {
            p_y = p.indexOf(x);
            y_values.add(p.indexOf(p_y+1)+1);
        }
        return y_values;
```

> 참고 사이트 <br>
> [Java - ArrayList.indexOf() 사용 방법 및 예제](https://codechacha.com/ko/java-collections-arraylist-indexof/) <br>


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
     * Complete the 'permutationEquation' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts INTEGER_ARRAY p as parameter.
     */

    public static List<Integer> permutationEquation(List<Integer> p) {
    // Write your code here
        int p_y = 0;
        List<Integer> y_values = new ArrayList<>();
        for (int x=1; x<=p.size(); x++) {
            p_y = p.indexOf(x);
            y_values.add(p.indexOf(p_y+1)+1);
        }
        return y_values;
    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int n = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> p = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.permutationEquation(p);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```

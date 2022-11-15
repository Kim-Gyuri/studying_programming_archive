## 문제
+ 0~100 범위 안의 점수를 얻는다.
+ 38점 아래값인 경우, failing grade이므로 값을 그대로 return해준다.
+ input grade의 5의 배수 사이값 차이가 3보다 적으면, input grade의 다음 5배수값을 return해준다.
+ 적절한 반올림한 값을 return해야 한다.

### sample 
```
# Input Format
The first line contains a single integer n, the number of students.
Each line i of the n subsequent lines contains a single integer, grades[i].


# Constraints
1 ≤ n ≤ 60
0 ≤ grades[i] ≤ 100



# Sample Input 0
4     // n = 4
73    // grade[i] ..
67
38
33



# Sample Output 0
75
67
40
33



# Explanation 0
(1) 73 -> 75
73 -> 5의 배수 사이값은 70 - 75이다. 
75-73= 2 < 3 이므로 반올림값 75를 받는다.

(2) 67 -> 67
67 -> 5의 배수값 65 - 70이다.
70-67= 3 == 3 이므로 반올림없이 67를 받는다.

(3) 38 -> 40
38 -> 5의 배수값 35 - 40이다.
40-38=2 < 3 이므로 반올림값 40를 받는다.

(4) 33 -> 33
38보다 작은 값은 그대로 return
```

## 풀이
+ g < 38인 값은 그대로 g을 return
+ 5의 배수 사이값과 g 차이값이 3보다 작은 경우, 반올림하여 return
+ `경우1`, g < 38 || (g%5) < 3 인 경우, g = g

```
# 예시로, g = 73, 38인 경우
73 -> 75
38 -> 40

67 -> 67

# 규칙을 보자면, (5로 나누었을 때 나머지)
73 -> 3
38 -> 3

67 -> 2


# g < 38 || (g%5) < 3 인 경우, g = g; (g값 그대로 return)
```

<br> <br>

+ `경우2`, g < 38 || (g%5) < 3 이 아닌 경우는 g의 다음 5의 배수값을 return해야 한다.

```
# 예시로, g = 73, 38인 경우
73 -> 75
38 -> 40

# 계산추적
73 + (2) = 75
(2)를 5기준으로 바꾸자.
(2) = (5 - 3) 
    = (5 - (70%5))
    
#그렇게, g + (5 - (g%5)) 해주면 된다.
```

<br> <br>

+ 삼항연산자로 위의 경우 2가지를 판단한다.
+ 받은 g를 List에 넣어서 결과를 return해준다.

```
    List<Integer> result = new ArrayList<>(); #결과를 반환할 List 생성
    for (Integer g : grades) { 
     g = g < 38 || (g % 5) < 3 ? g : g + 5 - (g % 5); # 삼항연산자로 경우2가지를 판단
    result.add(g); # 결과를 저장해 반환한다.
    }
    return result;
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
     * Complete the 'gradingStudents' function below.
     *
     * The function is expected to return an INTEGER_ARRAY.
     * The function accepts INTEGER_ARRAY grades as parameter.
     */

    public static List<Integer> gradingStudents(List<Integer> grades) {
    // Write your code here
    List<Integer> result = new ArrayList<>();
    for (Integer g : grades) {
     g = g < 38 || (g % 5) < 3 ? g : g + 5 - (g % 5);
    result.add(g);
    }
    return result;

    }

}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int gradesCount = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> grades = IntStream.range(0, gradesCount).mapToObj(i -> {
            try {
                return bufferedReader.readLine().replaceAll("\\s+$", "");
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .map(String::trim)
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> result = Result.gradingStudents(grades);

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


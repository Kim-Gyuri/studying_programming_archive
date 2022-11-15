## 문제
```
# Input
s, t : Sam's house location start-end point
a, b : Apple tree location, Orange tree location
m    : Apple array size (at which each A falls from the tree.)
n    : Orange array size (at which each A falls from the tree.)
m[i] : distances at which each fruits falls from the tree.
n[i] : distances at which each fruits falls from the tree.

# Output
each fruit falls within the region between Sam's house 


# 요약
Sam's house location 안에 포함되는 각각 떨어진 과일의 개수를 출력한다.
```

### Sample
```
# Sample Input 0
7 11
5 15
3 2
-2 2 1
5 -6


# Sample Output 0
1
1


# Explation 0
Sam's hose location start-end point : 7 ~ 11
Apple tree  info [location] [positions] : [5] [-2, 2, 1]
Orange tree info [location] [positions] : [15] [5, -6]

Apple : 5-2, 5+2, 5+1 이므로 7~11 범위 안에 들어오는 것은 5+2으로 1개이다.
Orange: 15+5, 15-6 이므로 7~11 범위 안에 들어오는 것은 15-6으로 1개이다.
```

## 풀이
+ Sam's 범위 안에 들어오는 과일이 있는지 판단하면 된다.
```
s ≤ (a+m[i]) ≤ t
s ≤ (b+m[i]) ≤ t
위의 식을 만족하는지 판단하면 된다.
```

<br><br>

+ List를 for() 돌려 if()조건을 만족하는지 판단한다.
```
List<Integer>로 과일 위치를 받아 판단한다.
if ()조건을 만족한 경우를 카운팅하여 return한다.

getHouse(int s, int t, int a, List<Integer> list) {
  int result = 0;
  for (Integer i : list) {
    if ((i+a) >= s) && ((i+a) <= t)) result++;
  }
  return result;
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
     * Complete the 'countApplesAndOranges' function below.
     *
     * The function accepts following parameters:
     *  1. INTEGER s
     *  2. INTEGER t
     *  3. INTEGER a
     *  4. INTEGER b
     *  5. INTEGER_ARRAY apples
     *  6. INTEGER_ARRAY oranges
     */

    public static void countApplesAndOranges(int s, int t, int a, int b, List<Integer> apples, List<Integer> oranges) {
    // Write your code here
    System.out.println(getHouse(s, t, a, apples));
    System.out.println(getHouse(s, t, b, oranges));
    

    }
    public static int getHouse(int s, int t, int a, List<Integer> list) {
        int result = 0;
        for (Integer i : list) {
           if (((i+a)>=s) && ((i+a)<=t)) result++;
        }
        return result;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));

        String[] firstMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int s = Integer.parseInt(firstMultipleInput[0]);

        int t = Integer.parseInt(firstMultipleInput[1]);

        String[] secondMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int a = Integer.parseInt(secondMultipleInput[0]);

        int b = Integer.parseInt(secondMultipleInput[1]);

        String[] thirdMultipleInput = bufferedReader.readLine().replaceAll("\\s+$", "").split(" ");

        int m = Integer.parseInt(thirdMultipleInput[0]);

        int n = Integer.parseInt(thirdMultipleInput[1]);

        List<Integer> apples = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        List<Integer> oranges = Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
            .map(Integer::parseInt)
            .collect(toList());

        Result.countApplesAndOranges(s, t, a, b, apples, oranges);

        bufferedReader.close();
    }
}
```

## 문제
입력받은 3x3 배열을  마방진으로 바꾸었을 때, 필요한 비용을 출력하라.

### Sample
```
# Sample Input 0
4 9 2
3 5 7
8 1 5


# Sample Output 0
1



# Explanation
입력받은 배열을 아래의 마방진으로 변경했을 때,
4 9 2
3 5 7
8 1 6

5->6만 변경하면 된다. 
cost = |5-6|
```

## 풀이
### 마방진이란
```
조건 1) 가로, 세로, 대각선의 합이 모두 같다.
조건 2) 1 ~ 9의 수들을 모두 배치해야 하므로 이들을 싹 더하면 45가 된다.
조건 3) 1 ~ 9의 수들로 배치했을 때, 가로, 세로, 대각선에 해당하는 줄의 합이 15이 된다.
조건 4) 한 줄에서 배치하는 숫자는 3개이므로, 45에서 3을 나누면 15가 된다.
```

<br> <br>

### 3x3 마방진
```
# 3개의 숫자로 15가 되는 경우의 수
9 + 1 + 5 = 15
9 + 2 + 4 = 15
8 + 1 + 6 = 15
8 + 2 + 5 = 15
8 + 3 + 4 = 15
7 + 2 + 6 = 15
7 + 3 + 5 = 15
6 + 4 + 5 = 15




# 3x3 마방진은 아래와 같이 8가지가 있다.

816	  438	  294	  672   618   834	  492	  276
357	  951	  753	  159   753	  159	  357	  951
492	  276	  618	  834   294	  672	  816	  438
```

<br><br>

### 마방진 포인트
```
1) 5는 중앙에 있어야 한다.
9 + 5 + 1
8 + 5 + 2
7 + 5 + 3
6 + 5 + 4


2) 1-5-9와 3-5-7은 십자가모양이 고정이다.
   3         7          3         7         9          1        9        1
 9 5 1     9 5 1      1 5 9     1 5 9     3 5 7      3 5 7    7 5 3    7 5 3
   7         3          7         3         1          9        1        9
  
  
3) 홀수를 먼저 고정해놓고 짝수를 채운다.  
```

<br> <br>

### 코드 스케치
```
1) 마방진의 모서리는 짝수로 채운다.
2) 5는 중앙에 있어야 한다.
3) 5를 제외한 [1, 2, 3, 4, 6, 7, 8, 9]의 조합으로 나올 수 있는 행렬의 갯수는 8!이다.


#1 perm 함수
: {1, 2, 3, 4, 6, 7, 8, 9}의 조합으로 나올 수 있는 행렬을 만들어보자.


#2 makeMagicSquare() 
if()문으로 가로,세로, 대간선의 합이 15인 행렬을 뽑는다.
가로 줄 1 a.get(0) + a.get(1) + a.get(2)
가로 줄 2 a.get(3) + a.get(4) + a.get(5)
가로 줄 3 a.get(6) + a.get(7) + a.get(8)
세로 줄 1 a.get(0) + a.get(3) + a.get(6)
세로 줄 2 a.get(1) + a.get(4) + a.get(7)
세로 줄 3 a.get(2) + a.get(5) + a.get(8)
왼-오 대각선 a.get(0) + a.get(4) + a.get(8)
오-왼 대각선 a.get(2) + a.get(4) + a.get(6)

List<List<Integer>> magicSquare = new ArrayList<>();
        for (List<Integer> a : permList) {
              if ((a.get(0) + a.get(1) + a.get(2)) == 15 &&
                    (a.get(3) + a.get(4) + a.get(5)) == 15 &&
                    (a.get(6) + a.get(7) + a.get(8)) == 15 &&
                    (a.get(0) + a.get(3) + a.get(6)) == 15 &&
                    (a.get(1) + a.get(4) + a.get(7)) == 15 &&
                    (a.get(2) + a.get(5) + a.get(8)) == 15 &&
                    (a.get(0) + a.get(4) + a.get(8)) == 15 &&
                    (a.get(2) + a.get(4) + a.get(6)) == 15) {
                        magicSquare.add(a);
                }
        }




#3 cost 계산
the minimal total cost of converting the input square to a magic square
makeMagicSquare()에서 구한 마방진의 개수만큼 for()문을 돌아 구한다.

## 3x3 행렬의 차이값은
모든 차이가 가장 작은 1,
가장 큰 숫자 9와의의 차이가 8만큼 난다고 가정했을 때,
the minimal total cost  = 72 ( 9칸 모두 8만큼 차이가 난다, 9*8)

## for()문 돌면서 구하면 된다.
int minCost = 72;
for (i..)
 for (j..) 
    cost += Math.abs(입력 - 3x3행렬 값 차이)
 
 if (cost < minCost) minCost = cost;
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
     * Complete the 'formingMagicSquare' function below.
     *
     * The function is expected to return an INTEGER.
     * The function accepts 2D_INTEGER_ARRAY s as parameter.
     */

    public static int formingMagicSquare(List<List<Integer>> s) {
    // Write your code here
        int minCost = 72;
        
        List<List<Integer>> magicSquare = makeMagicSquare();
        for (List<Integer> square : magicSquare) {
            int cost = 0;
            for (int i=0; i<3; i++) {
                for (int j=0; j<3; j++) {
                    cost += Math.abs(s.get(i).get(j) - square.remove(0));
                }
            }
            if (cost < minCost) minCost = cost;
        }
        return minCost;
    }
    
    private static List<List<Integer>> makeMagicSquare() {
        int arr[] = {1,2,3,4,5,6,7,8,9};
        List<List<Integer>> permList = new ArrayList<>();
        perm(permList, arr, 0);
        
        for (List<Integer> a : permList) {
            a.add(4,5);
        }
        
        List<List<Integer>> magicSquare = new ArrayList<>();
        for (List<Integer> a : permList) {
              if ((a.get(0) + a.get(1) + a.get(2)) == 15 &&
                    (a.get(3) + a.get(4) + a.get(5)) == 15 &&
                    (a.get(6) + a.get(7) + a.get(8)) == 15 &&
                    (a.get(0) + a.get(3) + a.get(6)) == 15 &&
                    (a.get(1) + a.get(4) + a.get(7)) == 15 &&
                    (a.get(2) + a.get(5) + a.get(8)) == 15 &&
                    (a.get(0) + a.get(4) + a.get(8)) == 15 &&
                    (a.get(2) + a.get(4) + a.get(6)) == 15) {
                        magicSquare.add(a);
                }
        }
        return magicSquare;
    }

    private static void perm(List<List<Integer>> permList, int[] arr, int pivot) {
        if (pivot == arr.length) {
            List<Integer> a = new ArrayList<>();
            for (Integer i :arr) a.add(i);
            permList.add(a);
            return;
        }
        for (int i=pivot; i<arr.length; i++) {
            swap(arr, i, pivot);
            perm(permList, arr, pivot+1);
            swap(arr, i, pivot);
        }
    }
    
    private static void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        List<List<Integer>> s = new ArrayList<>();

        IntStream.range(0, 3).forEach(i -> {
            try {
                s.add(
                    Stream.of(bufferedReader.readLine().replaceAll("\\s+$", "").split(" "))
                        .map(Integer::parseInt)
                        .collect(toList())
                );
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        });

        int result = Result.formingMagicSquare(s);

        bufferedWriter.write(String.valueOf(result));
        bufferedWriter.newLine();

        bufferedReader.close();
        bufferedWriter.close();
    }
}
```



> [참고 사이트](https://ejiaah.com/algorithm/hackerrank/hackerrank-Forming-a-Magic-Square/) <br>
> 마방진 이론을 알 수 있어야 접근할 수 있었던 문제였다. (어려움)

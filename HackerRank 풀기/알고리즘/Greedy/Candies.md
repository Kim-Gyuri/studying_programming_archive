## 문제
유치원 아이들에게 사탕을 나누어 주려고 한다. 나누어 줄 수 있는 사탕 개수의 최솟값은?

+ Function Description
```
Complete the candies function in the editor below.
candies has the following parameter(s):

int n: the number of children in the class
int arr[n]: the ratings of each student
```

+ Returns
```
int: the minimum number of candies Alice must buy
```

### 포인트
+ public static long candies(int n, List<Integer> arr): <br> 문제에서 반환타입을 long으로 정했으므로, 오류나지 않도록 주의하자. 
+ Alice wants to give at least 1 candy to each child. : <br> (최소한 1개씩은 할당한다.)
+ If two children sit next to each other, then the one with the higher rating must get more candies. : <br> index+1을 할 때,  더 높은 등급을 받은 아이는 더 많은 사탕을 받아야 한다.
  
## 풀이
+ 이번에도 문제 코드를 변형해서 풀었다. <br> :입력받기 (stream()없이 bufferedReader만 사용),  <br> Result.candies() 메소드 구현을 내부 메소드로 바꾸었다.
  
 <br> 
  
+ minimalCandies()
```
        long sumOfCandies = 0;  #결과값을 long타입으로 선언
        int[] candies = new int[n]; #캔디를 배열로 선언, 크기는 children num
        
        //seed
        candies[0] = 1; #캔디 최소 1개씩 할당이므로 
        
        //search forward sequences  #rating이 오름차순인 경우를 판단
        for (int i=1; i<n; i++) { 
            candies[i] = 1; #일단 1개를 할당하고
            if (children[i] > children[i-1]) { #next children이 더 높은 등급이면, 캔디 +1 하나 더 할당
                candies[i] = candies[i-1] + 1;
            }
        }
        
        // search reverse sequences  #rating이 내림차순인 경우도 판단
        # 예: 5 4 3 2 1
              가장 왼쪽의 아이가 5개 사탕을 받을 수 있다.
        for (int i=n-1; i>0; i--) {
            if (children[i] < children[i-1]) candies[i-1] = Math.max(candies[i-1], candies[i]+1);
        }
        
        for (int i : candies) {  #총 캔디 개수를 계산
            sumOfCandies += i;
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


public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());
        
        int[] children = new int[n];
        for (int i=0; i<children.length; i++) {
            children[i] = Integer.parseInt(br.readLine());
        }
        
        System.out.println(minimalCandies(n, children));
        
    }
    
        
    private static long minimalCandies(int n, int[] children) {
        long sumOfCandies = 0;
        int[] candies = new int[n];
        
        //seed
        candies[0] = 1;
        
        //search forward sequences
        for (int i=1; i<n; i++) {
            candies[i] = 1;
            if (children[i] > children[i-1]) {
                candies[i] = candies[i-1] + 1;
            }
        }
        
        // search reverse sequences
        for (int i=n-1; i>0; i--) {
            if (children[i] < children[i-1]) candies[i-1] = Math.max(candies[i-1], candies[i]+1);
        }
        
        for (int i : candies) {
            sumOfCandies += i;
        }
        
        return sumOfCandies;
    }
}
```

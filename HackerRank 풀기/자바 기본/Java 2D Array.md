## 문제
+ 6x6 2차원 배열 입력을 받는다.
+ 배열 속에서 hourglass를 찾는데, 해당 hourglass의 요소를 더하면 가장 큰 수가 나온다.
+ 해당 가장 큰 수를 출력한다.

> hourglass는 다음과 같이 생겼다.
```java
1 1 1     1 1 0     1 0 0  #위
  1         0         0    #가운데
1 1 1     1 1 0     1 0 0  #아래
```

## 풀이
+ 해당 hourglass는 다음과 같이, 위, 가운데, 아래로 2차원 배열로 수식할 수 있다.
```
        #위
        sum += data[x-1][y-1];
        sum += data[x-1][y];
        sum += data[x-1][y+1];
        
        #가운데
        sum += data[x][y];
        
        #아래
        sum += data[x+1][y-1];
        sum += data[x+1][y];
        sum += data[x+1][y+1];
```        

```
# 1을 기준으로 이렇게 되므로,
000   (x,y)     (x,y+1)     (x,y+2)
 0              (x+1,y+1)
000   (x+2,y)   (x+2,y+1)   (x+2,y+2)


# 다시, 0을 기준으로 바꿔 getHour(int x, int y)를 만든다.
1 기준 좌표에 -1를 해주면 된다.
```
> [자바 2차원 배열 참고 사이트](https://hunit.tistory.com/164) <br>

<br> <br>

+ 최대 경우 수 비교하기
```
포인트: max = Integer.MIN_VALUE;와 비교한다.
# 0을 기준으로 비교한다면
-> 여기서 hourglass는 3*3이므로, 6-2해주면 0<i<4가 되므로 3*3을 확인할 수 있다.
for (int i=0 i<data.length-2; i++)
  for (int j=0; j<data.length-2; j++)

# getHour()에 넣어 사용하기 위해 i,j 시작값을 1로 맞춘다.
  
# max와 cur(현재값)을 비교해가며 최대 경우 수를 반환받기
                cur = getHour(i, j);
                if (cur > max)
                    max = cur;
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
    
    static int[][] data;
    
    static int getHour(int x, int y) {
        int sum = 0;
        sum += data[x-1][y-1];
        sum += data[x-1][y];
        sum += data[x-1][y+1];
        
        sum += data[x][y];
        
        sum += data[x+1][y-1];
        sum += data[x+1][y];
        sum += data[x+1][y+1];
        return sum;
    }
    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        data = new int[6][6];
        for (int i=0; i<6; i++) {
            for (int j=0; j<6; j++) {
                data[i][j] = sc.nextInt();
            }
        }
        int max = Integer.MIN_VALUE;
        int cur;
        for (int i=1; i<data.length-1; i++) {
            for (int j=1; j<data[i].length-1; j++) {
                cur = getHour(i, j);
                if (cur > max)
                    max = cur;
            }
        }
        System.out.println(max);
    }
}
```

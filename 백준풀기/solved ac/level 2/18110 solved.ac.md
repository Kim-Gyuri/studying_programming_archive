## 18110 solved.ac 
> 백준 문제 링크는 [여기](https://www.acmicpc.net/problem/18110)!

### 문제 포인트
문제의 난이도를 출력한다.
+  아직 아무 `의견이 없다면 문제의 난이도는 0`으로 결정한다.
+  의견이 하나 이상 있다면, 문제의 난이도는 모든 사람의 난이도 의견의 `30% 절사평균으로 결정`한다.
+  절사평균이란 극단적인 값들이 평균을 왜곡하는 것을 막기 위해 가장 큰 값들과 가장 작은 값들을 제외하고 평균을 내는 것을 말한다. 
+  `30% 절사평균의 경우 위에서 15%, 아래에서 15%를 각각 제외하고 평균`을 계산한다. 
+ 마지막으로, `계산된 평균도 정수로 반올림`된다.

### 예시
입력이 아래와 같이 주어졌을 때, 문제풀이를 정리해보자. <br>
```
# 1) 입력
5  # 난이도 의견의 개수 n
1  # n 줄의 사용자가 제출한 난이도
5
5
7
8
```

```
# 2) 풀이
[1] 5명이므로 15%는 0.75다. 따라서 반올림하면 1명이다.
      정렬했을 때 {1,5,5,7,8}으로 위 아래로 1,8을 제외시킨다.
> 5명의 30%  <-> " 위 아래로 (5 * 0.15) 명을 각각 제외시켜야 한다."


[2] {5,5,7}에서 평균을 구한다.
       이때 총합은 double로 구하고, 평균을 구하여 반올림해준다.



[3] 절사평균 구하는 식은 다음과 같다.
       위 아래로 있으므로 out * 2를 해준다.
int out = (int) Math.round(n * 0.15);
result = (int) Math.round(sum / (n - out * 2));   -> result = 6
```

<br>

### 풀이 포인트
+ `1` 위, 아래에서 절사해야 할 인원을 구한다.
> n * 0.15를 한 값에 반올림(round)를 한 만큼 절사한다.

+ `2` 주어진 의견들을 오름차순으로 정렬한다.

+ `3` 합을 구합니다. 이때 합을 담을 변수는 double 형으로 선언한다.

+ `4`  합을 구한 의견 수는 n-num*2 이므로, 
      sum / (n-num*2) 에서 반올림(round)을 해준다.


<br>

### 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine());

        int[] nums = new int[n];
        for (int i=0; i<n; i++) {
            nums[i] = Integer.parseInt(br.readLine());
        }
        Arrays.sort(nums);

        int out = (int) Math.round(n * 0.15);
        double sum = 0;

        for (int i = out; i < n - out; i++) {
            sum += nums[i];
        }

        int result = (int) Math.round(sum / (n - out * 2));
        System.out.println(result);
    }
}
```

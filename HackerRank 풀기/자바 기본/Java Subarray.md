## 문제
`입력` <br>
+ 배열크기
+ 배열 요소들

`출력` <br>
+  연결되는 모든 subarray의 합을 검사하고 합이 음수인 경우 수를 출력한다.

## 검사하는 경우 수
```
# 예시 입력이 다음과 같을 때,
5
1 -2 4 -5 1

# 끊어주는 경우 수는 아래와 같다.
요소 값에 따라 다음과 같이 값이 나오는데, 값이 음수인 경우를 걸러내야 한다.
[1] = 1
[1,-2] = -1
[1,-2,4] = 3
[1,-2,4,-5] = -2
[1,-2,4,-5,1] = -1

[-2] = -2
[-2,4] = 2
[-2,4,-5] = -3
[-2,4,-5,1] = -2

[4] = 4
[4, -5] = -1
[4, -5, 1] = 0

[-5] = -5
[-5, 1] = -4
```

## sumArray() 구현
```
# for()문을 통해, 모든 경우의 수를 나눈다.
for(a ~ length)
 for(b=a length)
  for (c=a; c<=b; c=c+1)
이렇게 하면, 인덱스를 아래와 같이 볼 수 있다.
c = 0
c = 0 1
c = 0 1 2
c = 0 1 2 3
...


# 경우의 수로 나눈 뒤, 해당 요소 합을 sum에 저장하고,
  if()문으로 합이 음수이라면 counter 수를 +1 해준다.
int counter = 0;  
for(a ~ length)
 for(b=a length)
  for (c=a; c<=b; c=c+1)  
    sum = sum + arr[c];
  if (sum < 0) counter = counter + 1;
```    

## 코드
```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        Scanner sc = new Scanner(System.in);
        int range = sc.nextInt();
        int[] elements = new int[range];
        for (int i=0; i<elements.length; i++) {
            elements[i] = sc.nextInt();
        }
        System.out.println(sumArray(elements));
    }
    public static int sumArray(int[] arr) {
        int counter = 0;
        for (int i=0; i<arr.length; i++) {
            for (int j=i; j<arr.length; j++) {
                int sum = 0;
                for (int k=i; k<=j; k=k+1) {
                    sum = sum + arr[k];
                }
                if (sum < 0) {
                    counter = counter + 1;
                }
            }
        }
        return counter;
    }
}
```

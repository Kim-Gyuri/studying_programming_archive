## 문제 포인트
### 루프를 활용해 해당 쿼리 수식을 구현하기 <br>

```
s0 = a + 2^0*b 
s1 = a + 2^0*b + 2^1*b
...
sn-1 = a + ... + 2^n-1*6
```

`어떻게 풀지?` <br>
+ a += b * (int) Math.pow(2,j);을 n번 반복하기.
+ 2^n 제곱은 pow()를 사용한다.


<br><br>

### 예시
+ 입력
```
2
0 2 10
5 3 5
```

<br>

+ 출력
```
2 6 14 30 62 126 254 510 1022 2046
8 14 26 50 98
```

## 코드
```java
import java.util.*;
import java.io.*;

class Solution{
    public static void main(String []argh){
        Scanner in = new Scanner(System.in);
        int t=in.nextInt();
        for(int i=0;i<t;i++){
            int a = in.nextInt();
            int b = in.nextInt();
            int n = in.nextInt();
            for (int j=0; j<n; j++) {
                a += (int) Math.pow(2,j) * b;
                System.out.print(a + " ");
            }
            System.out.println();
        }
        in.close();
    }
}
```

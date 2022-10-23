## 문제
HashSet에 해당 입력을 a b를 넣었을 때, 짝을 이루는 경우의 수를 출력하라.

> 예시
```
#Sample Input
5
john tom
john mary
john tom
mary anna
mary anna


#Sample Output
1
2
2
3
3
```

### `HashSet이란`
HashSet의 특징을 정리하면 다음과 같습니다.
+ 중복된 값을 허용하지 않음
+ 입력한 순서가 보장되지 않음
+ null을 값으로 허용

## 풀이
```
HashSet에 넣는다면,-------------
# i=0 경우
letf = [john]
right = [tom]

# i=1 경우
left = [john, null]
right = [tom, mary]

# i=2 경우
left = [john, null, null]
right = [tom, mary, null]

# i=3 경우
left = [john, mary, null, null]
right = [tom, mary, null, anna]

# i=4 경우
left = [john, mary, null, null, null]
right = [tom, mary, null, anna, null]




# 입력된 값을 (a,b)로 담으면,-------------
i=0 -> (j,t)
i=1 -> (j,t) (null,m)
i=2 -> (j,t) (null,m) 
i=3 -> (j,t) (null,m) (null,a)
i=4 -> (j,t) (null,m) (null,a) 
```

## 코드
```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {

 public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        int t = s.nextInt();
        String [] pair_left = new String[t];
        String [] pair_right = new String[t];
        
        for (int i = 0; i < t; i++) {
            pair_left[i] = s.next();
            pair_right[i] = s.next();
        }
   
   //Write your code here
        HashSet<String> pairs = new HashSet<>(t);
        for (int i=0; i<t; i++) {
            pairs.add("("+pair_left[i]+", " +pair_right[i]+")");
            System.out.println(pairs.size());
        }
   }
}
```

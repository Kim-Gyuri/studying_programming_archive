## 문제
`입력` <br>
+ 1줄: 리스트 크기 N
+ 2줄: 리스트 요소들
+ 3줄: Query 수 Q
+ 4줄: Query 입력 \n  x y or x

> `Query 명령` : INSERT, DELETE 2가지가 있다. <br>
> (INSERT \n x y ): 인덱스x에 y값을 넣어라. <br>
> (DELETE x): 인덱스 x의 값을 삭제하라.  <br>

<br>

> 예시 <br>
```
# Sample Input
5
12 0 1 78 12
2
Insert
5 23
Delete
0

# Sample Output
0 1 78 12 23
```

> [while()문 참고 사이트](https://javarayo.tistory.com/entry/Java%EC%9D%98-%EC%A0%95%EC%84%9D%EB%B0%98%EB%B3%B5%EB%AC%B8-while%EB%AC%B8-do-while%EB%AC%B8) <br>

## 코드
```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution. */
        List<Integer> list = new LinkedList<>();
        Scanner sc = new Scanner(System.in);
        
        int n = sc.nextInt();
        while(n-- > 0) {
            list.add(sc.nextInt());
        }
        
        int queryNum = sc.nextInt();
        while(queryNum-- > 0) {
            String q = sc.next();
            if (q.equals("Insert")) {
                int x = sc.nextInt();
                int y = sc.nextInt();
                list.add(x,y);
            } else {
                int index = sc.nextInt();
                list.remove(index);
            }
        }
        for (Integer i : list) {
            System.out.print(i + " ");
        }
    }
}
```

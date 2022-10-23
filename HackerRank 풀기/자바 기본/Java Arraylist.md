## 문제
`입력` <br>
+ 1줄 : 전체 ArrayList 크기
+ 2줄~ : ArrayList의 line번호, 해당 line의 요소값들
+ 다음 줄 : query 크기
+ 다음 줄 : query 값들 (line번호, 해당 line의 index번호)

```
# Sample Input

5
5 41 77 74 22 44
1 12
4 37 34 36 52
0
3 20 22 33
5
1 3
3 4
3 1
4 3
5 5
```

![java ArrayList](https://user-images.githubusercontent.com/57389368/197384922-d88c492a-3984-42f8-8134-8d1eee58e28e.JPG) <br>

## 구현
```
# 입력받기
List<List<Integer>> = ArrayList<...>로 사용하기
전체 ArrayList 안에 각각 line을 List<Integer>로 받아온다.
        List<List<Integer>> lineList = new ArrayList<List<Integer>>();
        int n = sc.nextInt();
        for (int i=0; i<n; i++) {
            ArrayList<Integer> line = new ArrayList<>();
            int d = sc.nextInt();
            for (int j=0; j<d; j++) {
                line.add(sc.nextInt());
            }
            lineList.add(line);
        }
        
# 쿼리 실행
-> 해당 쿼리는 1부터 시작한다. (index는 0부터 시작하므로 -1 해주어 맞춘다.)
int x, int y
System.out.println(lineList.get(x-1).get(y-1));
```

## 코드
```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) throws IOException {
        Scanner sc = new Scanner(System.in);
        
        List<List<Integer>> lineList = new ArrayList<List<Integer>>();
        int n = sc.nextInt();
        for (int i=0; i<n; i++) {
            ArrayList<Integer> line = new ArrayList<>();
            int d = sc.nextInt();
            for (int j=0; j<d; j++) {
                line.add(sc.nextInt());
            }
            lineList.add(line);
        }
        
        int q = sc.nextInt();
        for (int i=0; i<q; i++) {
            int x = sc.nextInt();
            int y = sc.nextInt();
            try {
                System.out.println(lineList.get(x-1).get(y-1));
            } catch (Exception e) {
                System.out.println("ERROR!");
            }
        }
        
    }
}
```

## 문제
game[] 배열로 구현하는 문제로, 다음과 같은 이동이 가능하다.
+ index 0에서부터 시작한다.
+ `뒤로 이동` <br> cell이 있고 0을 포함한 경우는 뒤로 walk 가능하다.
+ `앞으로 이동` <br> cell i+1에 0을 포함한 경우, i+1에 walk 가능하다. <br> cell i+leap에 0을 포함한 경우, i+leap로 jump 가능하다. <br> n-1 cell에 있거나, i+leap>=n인 경우 walk or jump 해서 array 끝에 도달해 게임 이길 수 있다.             
+ `other words` <br> 목적지 index가 cell 0을 포함한 경우, i에서 index i+1, i-1 or i+leap으로 이동 가능. <br> 목적지 index가 n-1보다 크다면, 게임에서 이길 수 있다.

<br>

`입력` <br> 
+ 1줄 : 전체 query 크기 
+ 2줄~ : game[] 크기, leap 크기 \n game[] 요소들 
> (q) <br> (n, leap) <br> game[] 요소들

`출력` <br>
game[]에서 이겼으면 YES, 졌으면 NO를 각 줄에 출력한다.

<br>

> 예시
```
# Sample Input

STDIN           Function
-----           --------
4               q = 4 (number of queries)
5 3             game[] size n = 5, leap = 3 (first query)
0 0 0 0 0       game = [0, 0, 0, 0, 0]
6 5             game[] size n = 6, leap = 5 (second query)
0 0 0 1 1 1     . . .
6 3
0 0 1 1 1 0
3 1
0 1 0


#Sample Output

YES
YES
NO
NO
```


> 분석
```
# game[] = [0 0 0 0 0], leap=3 인 경우
-> 모든 cell에 0이 포함되어 있기 때문에, walk jump 모두 가능해 return YES

#game[] = [0 0 0 1 1 1], leap=5인 경우
-> index 1로 walk or jump가 가능한데 i+leap=6이므로 game[] 끝까지 도달해 return YES

#game[] = [0 0 1 1 1 0] leap=3인 경우
-> 연속 세 개 연속 1은 건널 수 없다. return NO

#game[] = [0 1 0] leap=1인 경우
-> 인덱스1에서 더 이상 움직일 수 없다. return NO
```

## 풀기
```
# solve() 메소드로 구현해보기
포인트 : 'leap 만큼 점프를 recursion

## 기본 규칙
(1)
목적지 index가 n-1보다 크다면, 게임에서 이길 수 있다. ->  if (i >= game.length) return true;
(2)
예외 처리: i는 0부터 시작하고 항상 game[0]=0이어야 한다. ->  else if (i<0 || game[i]==1) return false;
(3)
현재 index가 0인데, 이것을 1로 바꿔서 다시 index로 오는 것을 방지한다. -> game[i]=1;
(4)
앞으로 이동하는 경우 수를 ||로 묶어 recursion 해준다. (*경우->  목적지 index가 cell 0을 포함한 경우, i에서 index i+1, i-1 or i+leap으로 이동 가능)

    private static boolean solve(int leap, int[] game, int i) {
        if (i >= game.length) return true;
        else if (i<0 || game[i]==1) return false;
        game[i] = 1;
        return solve(leap, game, i+1) || solve(leap, game, i-1) || solve(leap, game, i+leap);
    }
```

## 코드
```java
import java.util.*;

public class Solution {

    public static boolean canWin(int leap, int[] game) {
        // Return true if you can win the game; otherwise, return false.
        return solve(leap, game, 0);
    }
    private static boolean solve(int leap, int[] game, int i) {
        if (i >= game.length) return true;
        else if (i<0 || game[i]==1) return false;
        game[i] = 1;
        return solve(leap, game, i+1) || solve(leap, game, i-1) || solve(leap, game, i+leap);
    }

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int q = scan.nextInt();
        while (q-- > 0) {
            int n = scan.nextInt();
            int leap = scan.nextInt();
            
            int[] game = new int[n];
            for (int i = 0; i < n; i++) {
                game[i] = scan.nextInt();
            }

            System.out.println( (canWin(leap, game)) ? "YES" : "NO" );
        }
        scan.close();
    }
}
```

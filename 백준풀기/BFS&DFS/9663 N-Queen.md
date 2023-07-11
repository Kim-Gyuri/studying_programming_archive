[9663 N-Queen](https://www.acmicpc.net/problem/9663)를 풀자. <br>

### 문제
+ 크기가 N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.
+ N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성하시오.

<br>

### 풀이
+ `1` 첫번째 행의 열을 차례대로 반복문을 돌린다.
+ `2` 같은 행에는 1개의 퀸 밖에 못 두므로 끝 행까지 놓았다면 다 놓은 것이다. -> 카운팅 (result++)
+ `3` 일차원 배열로 풀기 위해 map[column] = row;로 dfs() 탐색한다.
+ `4` dfs()는 <br> 현재 column에서 퀸을 성공적으로 놓았을 경우, 다음 행을 검사하기 위해 호출한다. <br> 두번째 매개변수인 column은 퀸이 놓여져 있는 행을 전달한다.
+ `5` dfs()에서는 <br> 마지막 행까지 성공적으로 놓은 경우, result를 증가시켜준다. <br> 아닌 경우(성공적으로 놓지 못한 경우)는 해당 행의 열을 차례대로 검사하며 놓을 수 있다면 다음 행으로 dfs() 재귀호출
+ `6` check() 로직으로 해당 행열에 퀸을 놓을 수 있는지 체크해준다. <br>
  + 첫번째로 해당 열에 퀸이 놓아져 있는지 체크한다.
  + 두번째로 대각선을 검사한다. <br> ( 이 대각선 검사하는 수식을 알고 있어야 한다. -> <br> 현재좌표의 행과 열을 뺀 절대값과 비교좌표의 행과 열을 뺀 절대값이 같으면 서로 대각선에 있는 좌표다.)

> [참고 코드](https://youngest-programming.tistory.com/229)

<br>

### 코드
```java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static int result = 0; // 퀸을 놓을 수 있는 수 (결과값 담을 곳)
    static int N; // (입력, N*N 크기 체스판이다.)

    // 일차원 배열로 풀기 위해 map[column] = row;로 dfs() 탐색한다.
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        // 첫번째 행의 열을 차례대로 반복문을 돌린다.
        // -> 첫번째 행1부터 차례대로 N열까지, 퀸을 놓고 다음 행을 호출한다.
        for (int col=1; col<=N; col++) {
            int[] map = new int[N+1];
            map[1] = col;
            dfs(map, 1);
        }
        System.out.println(result); //N=8 -> result :92
    }

    private static void dfs(int[] map, int row) {
        if (row == N) { // 마지막 행까지 탐색을 마쳤다면,
            //마지막 행까지 성공적으로 놓은 경우, result++ 카운팅
            result++;
        }
        // (성공적으로 놓지 못한 경우)는
        else { //해당 행의 열을 처음부터 N까지 차례대로 탐색한다.
            for (int col=1; col<=N; col++) {
                map[row+1] = col;
                // 해당 행의 열을 차례대로 검사하며 놓을 수 있다면 다음 행으로 dfs() 재귀호출
                if (check(map, row+1)) {
                    dfs(map, row+1);
                }
            }
        }
    }

    //check(): 해당 행열에 퀸을 놓을 수 있는지 체크해준다.
    private static boolean check(int[] map, int row) {
        // 현재 퀸을 놓으려는 행의 이전 행들을 차례대로 검사한다.
        for (int i=1; i<row; i++) {
            // 이전 행들에 "같은 열"에 퀸이 있는 경우
            if (map[i] == map[row]) {
                return false; //-> 퀸을 놓을 수 없다.
            }
            // 이전 행들에 "대각선" 공격 가능한 퀸이 있는 경우
            if (Math.abs(i-row) == Math.abs(map[i] - map[row])) {
                return false; // -> 퀸을 놓을 수 없다.
            }
        }
        // for()돌아도 false가 아닌 경우, "퀸을 놓을 수 있는 곳"이다.
        return true;
    }
}
```










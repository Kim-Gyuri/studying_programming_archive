## 16929 Two Dots
> 문제 링크는 [여기](https://www.acmicpc.net/problem/16929)

### 문제
![image](https://github.com/Kim-Gyuri/studying_programming_archive/assets/57389368/5774278c-1302-4906-b73c-c5843f41a447)

게임판의 상태가 주어졌을 때, 사이클이 존재하는지 아닌지 구해보자. <br> 사이클이 존재하는 경우에는 "Yes", 없는 경우에는 "No"를 출력한다.
+ 모든 k개의 점은 서로 다르다. 
+ k는 4보다 크거나 같다.
+ 점의 색은 알파벳 대문자 한 글자이다.
+ 모든 점의 색은 같다.
+ 모든 1 ≤ i ≤ k-1에 대해서, di와 di+1은 인접하다. 또, dk와 d1도 인접해야 한다. 두 점이 인접하다는 것은 각각의 점이 들어있는 칸이 변을 공유한다는 의미이다.

### 풀이
dfs를 구현해야 하는 문제다. 자세한 설명은 코드 주석으로 적었다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static char[][] board;
    static boolean[][] visited;
    static int n, m;
    static int[] dx = {0,1,0,-1};
    static int[] dy = {1,0,-1,0};

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        n = Integer.parseInt(st.nextToken());
        m = Integer.parseInt(st.nextToken());
        board = new char[n+1][m+1];
        visited = new boolean[n+1][m+1];

        for (int i=1; i<=n; i++) {
            String str = br.readLine();
            for (int j=1; j<=m; j++) {
                board[i][j] = str.charAt(j-1);
            }
        }

        // 탐색 --
        for (int i=1; i<=n; i++) {
            for (int j=1; j<=m; j++) {
                boolean ans = false;
                // 방문하지 않은 좌표라면, dfs 탐색
                if (!visited[i][j]) {
                    ans = dfs(0, 0, i, j);
                }
                // 만약 사이클이 있다면 yes 출력하고 프로그램 종료한다.
                if (ans) {
                    System.out.println("Yes");
                    return;
                }
            }
        }
        // 모든 탐색을 마쳤는데 사이클이 없다면 No 출력
        System.out.println("No");
    }

    private static boolean dfs(int preX, int preY, int startX, int startY) {
        // 이미 방문했던 곳이므로 true 반환
        if (visited[startX][startY]) return true;
        // 현재 좌표를 방문 처리
        visited[startX][startY] = true;

        // dfs 탐색
        for (int i=0; i<dx.length; i++) {
            int x2 = startX + dx[i];
            int y2 = startY + dy[i];
            // 주어진 범위에서 이동
            if (x2 >= 1 && y2 >= 1 && x2 <= n && y2 <= m) {
                // 이전에 왔던 좌표가 아니면서, 현재 좌표와 이전 좌표의 색상이 같은 경우
                if ((x2!=preX || y2!=preY) && (board[x2][y2] == board[startX][startY])) {
                        // 현재 좌표를 시작점으로 dfs 재귀호출하여 사이클을 찾는다.
                    if (dfs(startX, startY, x2, y2)) {
                        // 사이클 찾았다면 true 반환
                        return true;
                    }
                }
            }
        }
        // 사이클이 없는 경우 false 반환
        return false;
    }
}
```

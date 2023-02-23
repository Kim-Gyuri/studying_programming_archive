## 문제
미로에서 1은 이동할 수 있는 칸을 나타내고, 0은 이동할 수 없는 칸을 나타낸다. <br>
이러한 미로가 주어졌을 때, (0, 0)에서 출발하여 (N, M)의 위치로 이동할 때 지나야 하는 (x,y)를 출력하고 최소의 칸 수를 구하자. <br>
한 칸에서 다른 칸으로 이동할 때, 서로 인접한 칸으로만 이동할 수 있다. <br>
참고로 (0,0) 칸은 왼쪽 모서리.

## 풀이 포인트
BFS를 돌리면서 동서남북을 찾는데 1칸 이동할 때마다 누적 이동수를 1씩 더해나가는 로직이 구현해야 한다. <br>
2차 배열 map을 만들어, 이동했을 때 map 값이 1이라면 해당 칸의 값에 이전 칸의 값의 +1을 넣어주면 된다. <br> 
Stack을 사용하여 (x,y) 값을 관리한다.

## 입력과 출력
```
[입력]--------------------
7 7      #[행x열]
1011111  #미로 값을 받는다.
1110001
1000001
1000001
1000001
1000001
1111111

[출력]---------------------
[x]0,[y]0 으로 이동했습니다.
[x]1,[y]0 으로 이동했습니다.
[x]2,[y]0 으로 이동했습니다.
[x]1,[y]1 으로 이동했습니다.
[x]1,[y]2 으로 이동했습니다.
[x]0,[y]2 으로 이동했습니다.
[x]0,[y]3 으로 이동했습니다.
[x]0,[y]4 으로 이동했습니다.
[x]0,[y]5 으로 이동했습니다.
[x]0,[y]6 으로 이동했습니다.
[x]1,[y]6 으로 이동했습니다.
[x]2,[y]6 으로 이동했습니다.
[x]3,[y]6 으로 이동했습니다.
[x]4,[y]6 으로 이동했습니다.
[x]5,[y]6 으로 이동했습니다.
[x]6,[y]6 으로 이동했습니다.
[x]6,[y]5 으로 이동했습니다.
[x]6,[y]4 으로 이동했습니다.
[x]6,[y]3 으로 이동했습니다.
[x]6,[y]2 으로 이동했습니다.
[x]6,[y]1 으로 이동했습니다.
[x]6,[y]0 으로 이동했습니다.
[x]5,[y]0 으로 이동했습니다.
[x]4,[y]0 으로 이동했습니다.
[x]3,[y]0 으로 이동했습니다.
총 15 이동했습니다.
```


## 코드
```java
package hackerrank;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Stack;
import java.util.StringTokenizer;

public class Main {
    private static int[][] map;
    private static boolean[][] visited;
    private static int weight;
    private static int height;
    private static int[] dx = {-1,1,0,0};
    private static int[] dy = {0,0,-1,1};

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()," ");

        height = Integer.parseInt(st.nextToken()); //행
        weight = Integer.parseInt(st.nextToken()); //열

        map = new int[height][weight];
        visited = new boolean[height][weight];
        for (int n=0; n<height; n++) {
            st = new StringTokenizer(br.readLine());
            String line = st.nextToken();
            for (int m=0; m<weight; m++) {
                map[n][m] = line.charAt(m) - '0';
            }
        }

        bfs(1,1);
        System.out.println("총 " + map[height-1][weight-1] + " 이동했습니다.");

    }
    private static void bfs(int x, int y) {
        Stack<Point> queue = new Stack<>();
        queue.push(new Point(x-1, y-1));
        System.out.println("[x]" + (x-1) + ",[y]" + (y-1) + " 으로 이동했습니다.");
        visited[x-1][y-1] = true;
        while(!queue.isEmpty()) {
            Point poll = queue.pop();
            for (int i=0; i<4; i++) {
                int x2 = poll.x + dx[i];
                int y2 = poll.y + dy[i];
                if (x2<0 || x2>=height || y2<0 || y2>=weight) continue;
                if (map[x2][y2] == 1 && !visited[x2][y2]) {
                    visited[x2][y2] = true;
                    map[x2][y2] = map[poll.x][poll.y] + 1; // 이전 좌표의 값의 +1을 가짐 (최소 거리 누적해서 더해나간다.)
                    queue.push(new Point(x2, y2));
                    System.out.println("[x]" + (x2) + ",[y]" + (y2)+ " 으로 이동했습니다.");
                }
            }
        }
    }
    static class Point {
        int x;
        int y;
        Point(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```

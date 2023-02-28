### 문제
[DFS와 BFS](https://www.acmicpc.net/problem/1260)를 풀어보았다. <br>
+ 그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오.
+ 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 
+ 정점 번호는 1번부터 N번까지이다.

<br>

`입력` <br>
+ 첫째 줄에, 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000) 그리고 탐색을 시작할 정점의 번호 V가 주어진다.
+ 다음 M개의 줄에는, 간선이 연결하는 두 정점의 번호가 주어진다. 

<br>

`출력` <br>
+ 첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다.

<br>

### 코드
```java
package hackerrank;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Collections;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    public static int startNode; // 시작 정점 번호
    public static int nodeCount; // 정점 수
    public static LinkedList<Integer>[] nodeList; // 인접리스트로 그래프를 만들었다.
    public static int edgeCount; // 간선 수
    public static StringBuilder dfsTemp = new StringBuilder(); // dfs 탐색결과를 저장
    public static StringBuilder bfsTemp = new StringBuilder(); // bfs 탐색결과를 저장

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        nodeCount = Integer.parseInt(st.nextToken());
        edgeCount = Integer.parseInt(st.nextToken());
        startNode = Integer.parseInt(st.nextToken());

        // 노드 리스트 세팅
        nodeList = new LinkedList[nodeCount+1];
        boolean[] bfsVisited = new boolean[nodeCount+1];
        boolean[] dfsVisited = new boolean[nodeCount+1];
        for (int i=0; i<=nodeCount; i++) {
            nodeList[i] = new LinkedList<>();
        }

        // 연결 리스트 간선 세팅 : 노드와 노드를 연결한다.
        for (int i=0; i<edgeCount; i++) {
            st = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(st.nextToken());
            int node2 = Integer.parseInt(st.nextToken());

            nodeList[node1].add(node2);
            nodeList[node2].add(node1);

            // 리스트 정렬 : 점이 여러 개인 경우에는 정점 번호가 작은 것부터 방문한다.
            Collections.sort(nodeList[node1]);
            Collections.sort(nodeList[node2]);
        }

        dfs(startNode, dfsVisited);
        bfs(startNode, bfsVisited);

        System.out.println(dfsTemp + "\n" + bfsTemp);
    }

    // 시작노드부터 연결된 끝노드까지 방문한다.
    private static void dfs(int currentNode, boolean[] visited) {
        if (visited[currentNode]) return;

        visited[currentNode] = true;
        dfsTemp.append(currentNode + " ");
        for (int nextNode : nodeList[currentNode]) {
            dfs(nextNode, visited);
        }
    }

    private static void bfs(int currentNode, boolean[] visited) {
        Queue<Integer> queue = new LinkedList<>(); // bfs이므로 Queue를 생성한다.
        queue.offer(currentNode); // 노드를 Queue에 넣는다.
        while (!queue.isEmpty()) { //Queue에 아무것도 남지 않을 때까지 반복한다.
            currentNode = queue.poll(); // 큐에서 데이터 하나를 꺼내서,
            if (visited[currentNode]) continue; // Queue에 추가했던 노드는 넘기고
            visited[currentNode] = true; //Queue에 추가되지 않았던 노드만 추가한다, 그리고 방문체크를 true로 바꾸고 bfsTemp에 추가한다.
            bfsTemp.append(currentNode +" ");
            queue.addAll(nodeList[currentNode]);
        }
    }

}
```

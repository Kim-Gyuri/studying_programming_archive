## BFS 너비 우선 탐색 (Breadth First Search)
+ 시작 정점으로부터 가까운 정점을 먼저 방문하고, 멀리 떨어져 있는 정점을 나중에 방문하는 알고리즘이다.
+ 두 노드 사이의 최단 경로 혹은 임의의 경로를 찾고 싶을 때 이 방법을 선택한다.
+ 그래프 탐색의 경우 어떤 노드를 방문했었는지 여부를 반드시 검사해야 한다. (이를 검사하지 않을 경우 무한루프에 빠질 위험이 있다.)
+ BFS는 재귀적으로 동작하지 않는다.
+ BFS는 방문한 노드들을 차례로 저장한 후 꺼낼 수 있는 자료 구조인 큐(Queue)를 사용한다.
+ 즉, 선입선출(FIFO) 원칙으로 탐색한다.
+ 일반적으로 큐를 이용해서 반복적 형태로 구현하는 것이 가장 잘 동작한다.

## 구현
+ 정점의 개수, 간선의 개수, 탐색을 시작할 정점을 번호를 입력받는다.
+ 노드 방문 여부를 검사하기 위한 boolean 배열도 선언한다.
+ 큐(Queue)를 이용한다.

### 구현 방법
+ 인접 리스트로 구현한 BFS
+ 인접 행렬로 구현한 BFS

<br>

### 예제 입력과 출력은 다음과 같다.
> `입력` 
```
4 5 1
1 2
1 3
1 4
2 4
3 4
```

> `출력`
```
bfs - 1 2 3 4
```

### 인접 리스트로 구현한 BFS
+ 처음 BFS 함수를 호출하면, 시작 정점의 번호를 넣어 탐색을 시작한다.
+ 큐를 생성하고 시작 정점 값을 큐에 넣는다.
+ 큐에서 정점을 꺼내, 이를 기준으로 간선이 연결되어 있고(인접하고), 아직 방문되지 않은 정점을 찾는다.
+ 조건에 맞는 정점을 찾는다면 해당 정점을 방문했음으로 표시 후, 큐에 넣는다.
+ 큐가 소진될 때까지 이를 반복하며 BFS를 수행하는 것이다.

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
    public static int nodeCount;
    public static int startNode;
    public static int edgeCount;
    public static LinkedList<Integer>[] nodeList;
    public static boolean[] bfsVisited;
    public static StringBuilder bfsTemp = new StringBuilder();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

       //정점개수, 간선개수, 탐색시작노드 세팅
        nodeCount = Integer.parseInt(st.nextToken());
        edgeCount = Integer.parseInt(st.nextToken());
        startNode = Integer.parseInt(st.nextToken());

       //노드리스트 세팅
        nodeList = new LinkedList[nodeCount+1];
        bfsVisited = new boolean[nodeCount+1];
        for (int i=0; i<=nodeCount; i++) {
            nodeList[i] = new LinkedList<>();
        }
        for (int i=0; i<edgeCount; i++) {
            st = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(st.nextToken());
            int node2 = Integer.parseInt(st.nextToken());

            nodeList[node1].add(node2);
            nodeList[node2].add(node1);
           //리스트 정렬 (점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문)
            Collections.sort(nodeList[node1]);
            Collections.sort(nodeList[node2]);
        }
      
      // bfs
        bfs(startNode, bfsVisited);
        System.out.println("bfs = " + bfsTemp);
    }

    public static void bfs(int currentNode, boolean[] visited) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(currentNode);
        while(!queue.isEmpty()) {
            currentNode = queue.poll();
            if (visited[currentNode]) continue;
            visited[currentNode] = true;
            bfsTemp.append(currentNode + " ");
            queue.addAll(nodeList[currentNode]);
        }
    }

}
```


### 인접 행렬로 구현한 BFS
```java
package hackerrank;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    public static int nodeCount;
    public static int startNode;
    public static int edgeCount;
    public static int[][] map;
    public static boolean[] bfsVisited;
    public static StringBuilder bfsTemp = new StringBuilder();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        nodeCount = Integer.parseInt(st.nextToken());
        edgeCount = Integer.parseInt(st.nextToken());
        startNode = Integer.parseInt(st.nextToken());

        map = new int[nodeCount+1][nodeCount+1]; // 인접배열
        bfsVisited = new boolean[nodeCount+1];

      //맵 세팅 (인덱스 1부터 세팅함)
      // 두 정점 사이에 여러 개의 간선이 있을 수 있다.
      // 입력으로 주어지는 간선은 양방향이다.
        for (int i=1; i<=edgeCount; i++) {
            st = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(st.nextToken());
            int node2 = Integer.parseInt(st.nextToken());
            map[node1][node2] = map[node2][node1] = 1;
        }
      //bfs 
        bfs(startNode);
        System.out.println("bfs = " + bfsTemp);
    }
  
    // BFS - 인접행렬
    public static void bfs(int currentNode) {
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(currentNode);
        bfsVisited[currentNode] = true;
        bfsTemp.append(currentNode + " ");
        while(!queue.isEmpty()) {
            currentNode = queue.poll();
           //현재 노드와 연결된 방문안된 노드들을 방문 후 큐에 추가
            for (int i=1; i<=nodeCount; i++) {
                if (map[currentNode][i] == 1 && !bfsVisited[i]) {
                    queue.offer(i);
                    bfsVisited[i] = true;
                    bfsTemp.append(i + " ");
                }
            }
        }
    }

}
```

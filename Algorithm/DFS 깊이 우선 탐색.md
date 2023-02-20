## DFS 깊이 우선 탐색 (Depth Fisrt Search)
+ 모든 노드를 방문하고자 하는 경우에 이 방법을 선택한다.
+ 완전 탐색 알고리즘에 자주 이용된다.
+ 자기 자신을 호출하는 순환 알고리즘의 형태
+ 래프 탐색의 경우 어떤 노드를 방문했었는지의 여부를 반드시 검사해야 한다. (안하면 무한루프 빠질 위험 있다.)

> [참고 사이트](https://minhamina.tistory.com/22) <br>

## DFS 인접 리스트로 구현하기
LinkedList에 정점과 간선을 넣어 인접 리스트를 만든다. <br>
+ 먼저 정점개수, 간선개수, 탐색시작노드 세팅해야 한다.
+ 그 다음, 노드리스트를 세팅한다.
+ (만약 방문 순서가 중요한 경우, 정점 번호가 작은 것부터 먼저 방문하도록 리스트 정렬한다.)
+ 재귀를 이용해 DFS 구현

### 입력
```
4 5 1  #[정점개수, 간선개수, 탐색시작노드]
1 2    #[노드 리스트]
1 3
1 4
2 4
3 4
```

### 출력
```
dfsTemp = 1 2 4 3 
3
```

### 코드
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Collections;
import java.util.LinkedList;
import java.util.StringTokenizer;

public class Main {
    static int startNode;
    static int nodeCount;
    static int edgeCount;
    static boolean[] visited;
    static StringBuilder dfsTemp = new StringBuilder();
    static LinkedList<Integer>[] nodeList;
    static int result;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        //정점개수, 간선개수, 탐색 시작노드 값 입력
        nodeCount = Integer.parseInt(st.nextToken());
        edgeCount = Integer.parseInt(st.nextToken());
        startNode = Integer.parseInt(st.nextToken());

        // 노드 리스트 입력
        visited = new boolean[nodeCount+1];
        nodeList = new LinkedList[nodeCount+1];
        for(int i=0; i<=nodeCount; i++) {
            nodeList[i] = new LinkedList<>();
        }
        for (int i=0; i<edgeCount; i++) {
            st = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(st.nextToken());
            int node2 = Integer.parseInt(st.nextToken());

            nodeList[node1].add(node2);
            nodeList[node2].add(node1);

            //리스트 정렬
            Collections.sort(nodeList[node1]);
            Collections.sort(nodeList[node2]);
        }

        dfs(startNode, visited);
        System.out.println("dfsTemp = " + dfsTemp);
        System.out.println(result-1);
    }

    // DFS - 인접리스트 - 재귀로 구현
    public static void dfs(int currentNode, boolean[] visited) {
        if (visited[currentNode]) return; //방문했던 노드는 지나가고,

        visited[currentNode] = true;   //정점을 방문하면 visited[v]에 true 값을 넣어 방문했다고 표시한 뒤
        dfsTemp.append(currentNode + " "); //dfsTemp에 방문한 정점을 저장한다.
        for (Integer nextNode : nodeList[currentNode]) { // 정점 인접리스트 순회 
            dfs(nextNode, visited);                      //// 방문하지 않은 정점이라면 다시 DFS
        }
        result++;  // 시작점에서 이어지는 노드를 카운트
    }
}
```

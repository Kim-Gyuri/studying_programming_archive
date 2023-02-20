# DFS 깊이 우선 탐색 (Depth Fisrt Search)
+ 모든 노드를 방문하고자 하는 경우에 이 방법을 선택한다.
+ 완전 탐색 알고리즘에 자주 이용된다.
+ 자기 자신을 호출하는 순환 알고리즘의 형태
+ 래프 탐색의 경우 어떤 노드를 방문했었는지의 여부를 반드시 검사해야 한다. (안하면 무한루프 빠질 위험 있다.)

> [참고 사이트](https://minhamina.tistory.com/22) <br>

### DFS 표현 변수
+ 정점의 개수, 간선의 개수, 탐색을 시작할 정점을 번호를 입력받는다.
+ 노드 방문 여부를 검사하기 위한 boolean 배열도 선언한다.

### DFS 구현 방법 2가지
+ 순환 호출 이용(재귀)
+ 명시적인 스택 사용 : 인접한 정점들을 스택에 저장하였다가 다시 꺼내어 작업한다.

# 구현
다음과 같은 방법이 있다.
+ 인접 리스트로 구현한 DFS
    + 재귀를 이용했을 때
+ 인접 행렬로 구현한 DFS
    + 재귀를 이용했을 때
    + 스택을 이용했을 때


<br>

### 다음과 같이 주어졌을 때
> `입력`
```
4 5 1  #[정점개수, 간선개수, 탐색시작노드]
1 2    #[노드 리스트]
1 3
1 4
2 4
3 4
```

> `출력`
```
dfsTemp = 1 2 4 3 
```

## 인접 리스트로 구현한 DFS
LinkedList에 정점과 간선을 넣어 인접 리스트를 만든다. <br>
+ 먼저 정점개수, 간선개수, 탐색시작노드 세팅해야 한다.
+ 그 다음, 노드리스트를 세팅한다.
+ (만약 방문 순서가 중요한 경우, 정점 번호가 작은 것부터 먼저 방문하도록 리스트 정렬한다.)
+ 재귀를 이용해 DFS 구현

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
        result++;  // 시작점에서 이어지는 노드를 카운트 -> result = 3;
    }
}
```

## 인접 행렬로 구현한 DFS
2차원 배열에 정점과 간선을 넣어 인접 행렬을 만든다.

 
### 재귀를 이용했을 때
+ 인접리스트에서 재귀를 이용해 구현했던 방법과 동일하다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    static int map[][];
    static boolean[] dfsVisited;
    static boolean[] bfsVisited;
    static StringBuilder dfsTemp = new StringBuilder();
    static int startNode;
    static int nodeCount;
    static int edgeCount;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        nodeCount = Integer.parseInt(st.nextToken());
        map = new int[nodeCount+1][nodeCount+1];
        dfsVisited = new boolean[nodeCount+1];

        edgeCount = Integer.parseInt(st.nextToken());
        startNode = Integer.parseInt(st.nextToken());

        //맵 세팅 (인덱스 1부터 세팅함)
        // 두 정점 사이에 여러 개의 간선이 있을 수 있다.
		// 입력으로 주어지는 간선은 양방향이다.
        for (int i=1; i<=edgeCount; i++) {
            st = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(st.nextToken());
            int node2 = Integer.parseInt(st.nextToken());
            map[node1][node2] = map[node2][node1] = 1;
        }

        dfs(startNode);
        System.out.println("dfs = "+ dfsTemp);
    }

   //DFS - 인접행렬 / 재귀로 구현 
    public static void dfs(int currentNode) {
        if (dfsVisited[currentNode]) return;

        dfsVisited[currentNode] = true;
        dfsTemp.append(currentNode+ " ");
        for (int i=1; i<=nodeCount; i++) {
           //연결되고 방문 안된 노드 탐색 (낮은 노드부터 방문됨)
            if (map[currentNode][i] == 1 && !dfsVisited[i]) dfs(i);
        }
    }

}
```

### 스택을 이용했을 때
+ 스택을 생성하고 탐색시작노드 값을 스택에 넣는다.
+ 스택의 top에 있는 정점을 기준으로 간선이 연결되어 있고(인접하고), 아직 방문되지 않은 정점을 찾는다.
+ 조건에 맞는 정점을 찾는다면 해당 정점을 방문했음으로 표시 후, 스택에 넣고 break를 건다.
> break를 걸어줌으로써, 찾은 정점을 기준으로 다시 탐색이 진행된다. <br>
> 이를 반복함으로써 DFS를 수행하는 것이다. <br>

+ 연결된 간선이 없고, 방문하지 않은 정점을 찾지 못한다면, 더이상 탐색할 수 없기에 pop()을 해 다시 돌아간다.
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    static int map[][];
    static boolean[] dfsVisited;
    static boolean[] bfsVisited;
    static StringBuilder dfsTemp = new StringBuilder();
    static int startNode;
    static int nodeCount;
    static int edgeCount;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        nodeCount = Integer.parseInt(st.nextToken());
        map = new int[nodeCount+1][nodeCount+1];
        dfsVisited = new boolean[nodeCount+1];

        edgeCount = Integer.parseInt(st.nextToken());
        startNode = Integer.parseInt(st.nextToken());

        //맵 세팅 (인덱스 1부터 세팅함)
        // 두 정점 사이에 여러 개의 간선이 있을 수 있다.
		// 입력으로 주어지는 간선은 양방향이다.
        for (int i=1; i<=edgeCount; i++) {
            st = new StringTokenizer(br.readLine());
            int node1 = Integer.parseInt(st.nextToken());
            int node2 = Integer.parseInt(st.nextToken());
            map[node1][node2] = map[node2][node1] = 1;
        }

        dfs_stack(startNode);
        System.out.println("dfs = "+ dfsTemp);
    }

   //DFS - 인접행렬 / 스택을 이용
    public static void dfs_stack(int currentNode) {
        Stack<Integer> stack = new Stack<>();
        stack.push(currentNode);
        dfsVisited[currentNode] = true;
        dfsTemp.append(currentNode + " ");

        while (!stack.isEmpty()) {
            currentNode = stack.peek();
            boolean flag = false;
            for (int i=1; i<=nodeCount; i++) {
                if (map[currentNode][i] == 1 && !dfsVisited[i]) {
                    stack.push(i);
                    dfsTemp.append(i + " ");
                    dfsVisited[i] = true;
                    flag = true;
                    break;
                }
            }
            if (!flag) stack.pop();
        }
    }

}
```

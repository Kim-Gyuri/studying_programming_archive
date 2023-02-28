# 📌 Graph
+ Graph란?
+ Undirected Graph와 Directed Graph
+ 가중치 그래프(Weight Graph)와 부분 그래프(Sub Graph)
+ 그래프를 구현하는 두 방법 (인접 행렬, 인접 리스트)
+ 그래프 탐색 - DFS, BSF 
+ Minimum Spanning Tree
+ 참고자료

## Graph란?
이전에 작성했던 [Graph에 대하여](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/16.%20Graph%EC%97%90%20%EB%8C%80%ED%95%B4%EC%84%9C.md) 글도 있다.
+ 트리 또한 그래프이며, 그 중 사이클이 허용되지 않는 그래프를 말한다.
+ 정점(Vertex)과 간선(Edge, 정점과 정점을 연결하는 선)의 집합이다.

## Undirected Graph와 Directed Graph
![방향그래프(Directed Graph)  무방향 그래프(Unidrect Graph)](https://user-images.githubusercontent.com/57389368/221765646-90dc2507-f2fa-45fa-9dfb-4f13eb368d48.PNG)
#### Undirected Graph는 edge가 방향이 없다.
#### Directed Graph는 edge의 방향이 있다.

> `Degree` <br>
> Undirected Graph에서 각 정점에 연결된 Edge의 개수를 Degree라고 한다. <br> Directed Graph에서는 간선에 방향성이 존재하기 때문에 Degree가 두 개로 나뉘게 된다. <br>
각 정점으로부터 나가는 간선의 개수를 Outdegree라 하고, 들어오는 간선의 개수를 Indegree라 한다.

## 가중치 그래프(Weight Graph)와 부분 그래프(Sub Graph)
![weighted-graph](https://user-images.githubusercontent.com/57389368/221767530-4cc0eede-d5a4-4197-b0c0-1cfcdc9a8e06.png) <br>
#### 가중치 그래프(Weight Graph) 
가중치 그래프란 간선에 가중치 정보를 두어서 구성한 그래프를 말한다. <br>
> 반대의 개념인 비가중치 그래프로, 모든 간선의 가중치가 동일한 그래프도 물론 존재한다. 

#### 부분 그래프(Sub Graph)
부분 집합과 유사한 개념, <br> 부분 그래프는 본래의 그래프의 일부 정점 및 간선으로 이루어진 그래프를 말한다.

## 그래프를 구현하는 두 방법 (인접 행렬, 인접 리스트)
#### 인접 행렬 (adjacent matrix) : 정방 행렬을 사용하는 방법
![adjacent matrix](https://user-images.githubusercontent.com/57389368/221769307-a16ed59f-8651-4beb-8863-6197017518be.PNG)

+ 2차원 배열에 표현
+ 각각의 노드들의 번호로 구성된 노드들을 테이블로 구성한다.
+ 서로 연결되면 1, 연결되지 않았으면 0으로 채운다.
+ 해당하는 위치의 value 값을 통해서 vertex 간의 연결 관계를 O(1)으로 파악할 수 있다. 
+ Edge 개수와는 무관하게 V^2 의 Space Complexity를 갖는다. 
+ Dense graph를 표현할 때 적절할 방법이다.

<br>

#### 인접 리스트 (adjacent list) : 연결 리스트를 사용하는 방법
![인접 리스트 (adjacent list)](https://user-images.githubusercontent.com/57389368/221769208-4cf1dfaf-f26d-4d04-9080-e1b20bad63d5.PNG)

+ 배열에 모든 노드를 넣는다.
+ 배열에 들어간 각 노드에 인접한 노드들을 LinkedList로 채워준다.
+ LinkedList를 채울 떄 순서는 상관없다.
+ List의 총 노드의 개수는 edge 개수의 2배.
+ vertex의 adjacent list를 확인해봐야 하므로 vertex 간 연결되어있는지 확인하는데 오래 걸린다. 
+ Space Complexity는 O(E + V)이다. Sparse graph를 표현하는데 적당한 방법이다.

## 그래프 탐색 - DFS, BFS
이전에 [Graph Search 방법 2가지](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/17.%20Graph%20Search.md)에 대한 글을 작성했었다. <br> 
#### 구현코드
+ [Graph search 구현](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/18.%20DFS%20Recursion.md) 코드도 작성했었다. <br> 
+ [DFS Recursion 구현](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/18.%20DFS%20Recursion.md) 코드도 작성했었다.
+ [Graph에서 두 개의 노드가 서로 만날 수 있는 경로가 있는지 확인하는 함수를 구현](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/19.%20Graph%EC%97%90%EC%84%9C%20%EB%91%90%20%EC%A7%80%EC%A0%90%EC%9D%98%20%EA%B2%BD%EB%A1%9C%EC%B0%BE%EA%B8%B0.md) 코드도 작성했었다.
+ [이진트리의 노드들을 각 레벨별로 LinkedList에 담는 알고리즘을 구현](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/21.%20%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC%EB%A5%BC%20%EB%A0%88%EB%B2%A8%EB%8B%A8%EC%9C%84%20%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A1%9C%20%EB%B3%80%ED%98%95%ED%95%98%EA%B8%B0.md) 코드도 작성했었다.


### 깊이 우선 탐색 (Depth First Search: DFS)
+ 그래프에서 연결된 정점이 있을 때까지 우선 탐색한다.
+ 마지막 노드를 만날 때까지 갔다가 다시 올라온다.
+ Stack 자료구조를 사용한다. 
+ Time Complexity : O(V+E) … vertex 개수 + edge 개수

### 그래프 탐색 - 너비 우선 탐색 (Breadth First Search: BFS)
+ 순서대로 레벨별 자식들을 탐색한다.
+ 자료구조로 Queue를 사용한다. 
+ Time Complexity : O(V+E) … vertex 개수 + edge 개수
+ 모든 간선에 가중치가 존재하지않거나 모든 간선의 가중치가 같은 경우, BFS로 구한 경로는 최단 경로이다.

## Minimum Spanning Tree
그래프 G 의 spanning tree 중 edge weight의 합이 최소인 spanning tree를 말한다. <br>
여기서 말하는 spanning tree란 그래프 G의 모든 vertex가 cycle이 없이 연결된 형태를 말한다.

## 참고자료
+ [DataStructure 면접준비 글 참고](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#graph)
+ [Graph 글 참고](https://poetic-code.tistory.com/88)
+ [그림 참고](https://chamdom.blog/graph/)
+ [엔지니어 대한민국 - 자바로 구현](https://www.youtube.com/@eleanorlim)

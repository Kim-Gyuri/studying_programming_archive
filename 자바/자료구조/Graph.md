# 📌 Graph
+ Graph란?
+ Undirected Graph와 Directed Graph
+ 가중치 그래프(Weight Graph)와 부분 그래프(Sub Graph)
+ 그래프를 구현하는 두 방법 (인접 행렬, 인접 리스트)
+ 그래프 탐색
+ 참고자료

## Graph란
+ 트리 또한 그래프이며, 그 중 사이클이 허용되지 않는 그래프를 말한다.
+ 정점(Vertex)과 간선(Edge, 정점과 정점을 연결하는 선)의 집합이다.

## Undirected Graph와 Directed Graph
![방향그래프(Directed Graph)  무방향 그래프(Unidrect Graph)](https://user-images.githubusercontent.com/57389368/221765646-90dc2507-f2fa-45fa-9dfb-4f13eb368d48.PNG)
#### Undirected Graph
edge가 방향이 없다.
#### Directed Graph
edge의 방향이 있다.

#### Degree
Undirected Graph에서 각 정점에 연결된 Edge의 개수를 Degree라고 한다. <br> Directed Graph에서는 간선에 방향성이 존재하기 때문에 Degree가 두 개로 나뉘게 된다. <br>
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


#### 인접 리스트 (adjacent list) : 연결 리스트를 사용하는 방법
![인접 리스트 (adjacent list)](https://user-images.githubusercontent.com/57389368/221769208-4cf1dfaf-f26d-4d04-9080-e1b20bad63d5.PNG)

+ 배열에 모든 노드를 넣는다.
+ 배열에 들어간 각 노드에 인접한 노드들을 LinkedList로 채워준다.
+ LinkedList를 채울 떄 순서는 상관없다.
+ List의 총 노드의 개수는 edge 개수의 2배.
+ vertex의 adjacent list를 확인해봐야 하므로 vertex 간 연결되어있는지 확인하는데 오래 걸린다. 
+ Space Complexity는 O(E + V)이다. Sparse graph를 표현하는데 적당한 방법이다.

## 그래프 탐색

## 참고자료
+ [DataStructure 면접준비 글 참고](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#graph)
+ [Graph 글 참고](https://poetic-code.tistory.com/88)
+ [그림 참고](https://chamdom.blog/graph/)

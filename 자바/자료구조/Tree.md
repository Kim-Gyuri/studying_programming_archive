# 📌 Tree
+ Tree의 정의
+ Tree의 관련용어
+ Binary Tree (이진 트리)
+ BST (Binary Search Tree)
+ 주어진 트리가 Binary 트리인지 확인하는 알고리즘 구현하기
  + using inorde
  + without array
  + max & min
    
+ 참고자료

## Tree의 정의
이전에 [Tree의 종류](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/13.%20Tree%EC%9D%98%20%EC%A2%85%EB%A5%98.md)에 대한 글을 작성했었다.

트리는 스택이나 큐와 같은 선형 구조가 아닌 비선형 자료구조이다. <br> 트리는 계층적 관계(부모-자식 구조)을 표현하는 자료구조이다.

## Tree 관련용어
![트리](https://user-images.githubusercontent.com/57389368/221153764-684d877d-0183-4e0a-be3d-ceb5434cdfd3.png)

+ Node : 트리를 구성하고 있는 각각의 요소
+ Edge : 노드와 노드를 연결하는 선 (branch 또는 link 라고도 부른다.)
+ Root Node : 최상위 노드
+ Terminal Node (leaf Node, 단말 노드) : 자식이 없는 노드
+ Internal Node (내부노드, 비단말 노드) : 단말 노드를 제외한 모든 노드로 루트 노드를 포함한다.
+ sibling(형제 노드) : 같은 부모를 가지는 노드.

## Binary Tree (이진 트리)
child node가 최대 2개까지 붙는다. <br>

<br>

트리에서는 각 `층별`로 숫자를 매겨서 이를 트리의 `Level(레벨)`이라고 한다. <br>
레벨의 값은 0 부터 시작하고 따라서 루트 노드의 레벨은 0 이다. 그리고 트리의 최고 레벨을 가리켜 해당 트리의 height(높이)라고 한다. <br>
> [이진트리를 레벨단위 리스트로 변형하기 - 구현코드](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/21.%20%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC%EB%A5%BC%20%EB%A0%88%EB%B2%A8%EB%8B%A8%EC%9C%84%20%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A1%9C%20%EB%B3%80%ED%98%95%ED%95%98%EA%B8%B0.md)를 했었다.         

<br>

> `Binary Tree 요약`  <br>
> 모든 레벨이 꽉 찬 이진 트리를 가리켜 포화 이진 트리(Perfect Binary Tree)라고 한다. <br>
> 위에서 아래로, 왼쪽에서 오른쪽으로 순서대로 차곡차곡 채워진 이진 트리를 가리켜 완전 이진 트리(Complete Binary Tree)라고 한다. <br>
> 모든 노드가 0개 혹은 2개의 자식 노드만을 갖는 이진 트리를 가리켜 정 이진 트리(Full Binary Tree)라고 한다. <br>

## BST (Binary Search Tree)
이진 탐색 트리는 이진 트리의 일종이다. 단 이진 탐색 트리에는 데이터를 저장하는 규칙이 있다. <br>
+ 이진 탐색 트리의 노드에 저장된 키는 유일하다.
+ 부모의 키가 왼쪽 자식 노드의 키보다 크다.
+ 부모의 키가 오른쪽 자식 노드의 키보다 작다.
+ 왼쪽과 오른쪽 서브트리도 이진 탐색 트리이다.

> `시간 복잡도` <br>
> ![BST](https://user-images.githubusercontent.com/57389368/221154942-86887f44-4499-448f-a621-4e56599911bb.png) <br>
>
> 이상적인 상황에서 탐색/삽입/삭제 모두 시간복잡도가 O(logN)이 된다. <br>
> 어떤 값 n을 찾을 때, 루트 노드와 비교해서 n이 더 작다면 루트 노드보다 큰 값들만 모여 있는 오른쪽 가지는 전혀 탐색할 필요가 없다. <br> 
> 마찬가지로 루트 노드의 왼쪽 자식보다 n이 크다면 왼쪽 자식의 왼쪽 가지는 탐색할 필요가 없고… <br>
> 다시 말해 트리 자체가 이진 탐색을 하기에 적합한 구성이 되는 것이다. 또한 값을 찾을 때뿐만이 아니라 값을 삽입하거나 삭제할 때도 똑같은 과정을 거친다. <br>
> <br>
> 값이 삽입되거나 삭제되는 경우에 따라서 운이 안좋으면 최악의 경우에 O(N)의 시간이 걸리게 된다.  <br>
> 예를 들어, 비어있는 이진 탐색 트리에 1부터 100까지 순서대로 삽입한다면 <br>
> 처음 루트 노드는 1이 되고, 2는 1보다 크니 1의 오른쪽 자식이 되고, 3은 1보다 크니 1의 오른쪽, 2보다 크니 2의 오른쪽…  <br>
> 이런 식으로 트리의 오른쪽 끝으로만 계속 성장하게 된다. <br>
> 이 상태로 50을 찾는다고 하면 결국 1부터 순서대로 오른쪽으로 쭈욱 내려가는 선형 탐색이나 다를게 없게 된다. <br>
> 이러한 경우를 트리가 편향(skew)되었다고 한다.

<br>

"최악의 경우에 O(N)의 시간이 걸리게 된다." <br>
배열보다 많은 메모리를 사용하며 데이터를 저장했지만 탐색에 필요한 시간 복잡도가 같게 되는 비효율적인 상황이 발생한다. <br>
이를 해결하기 위해 Rebalancing 기법이 등장했다. <br>
> 균형을 잡기 위한 트리 구조의 재조정을 Rebalancing이라 한다. <br>
> 이 기법을 구현한 트리에는 Red-Black Tree이다.


## 주어진 트리가 Binary 트리인지 확인하는 알고리즘 구현하기
```
어떤 노드를 봐도, 노드의 왼쪽트리들은 자기보다 작은 값을 가지고
노드의 오른쪽 트리들은 자기보다 큰 값을 가진다.
```
#### [방법1 - inorder (L, root, R)](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/23.%20%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC%EA%B2%80%EC%83%89%20-%201%20using%20inorder.md) <br>
#### [방법2 - without array](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/24.%20%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC%EA%B2%80%EC%83%89%20-%202%20%20without%20array.md) 
#### [방법3 - max & min](https://github.com/Kim-Gyuri/Java_datastructure_algorithm2/blob/master/%EB%85%B8%ED%8A%B8/25.%20%EC%9D%B4%EC%A7%84%ED%8A%B8%EB%A6%AC%EA%B2%80%EC%83%89%20-%203%20max%20%26%20min.md)
## 참고자료
[Tree란](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84)) <br>
[자료구조 참고글](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure#array-vs-linked-list) <br>

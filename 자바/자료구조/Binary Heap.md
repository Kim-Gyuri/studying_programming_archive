# 📌Binary Heap
+ Heap
+ Binary Heap
+ Heapify 구현하기

## Heap
힙(heap)은 최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 <br>
고안된 완전이진트리(complete binary tree)를 기본으로 한 자료구조이다.

> `complete binary tree` <br>
> 모든 노드들이 레벨별로 왼쪽부터 채워지면 된다. <br>
> 마지막 노드를 제외한 sub Tree의 레벨이 같아야 한다. <br>
> 마지막 노드는 왼쪽부터 채워져야 한다. <br>

<br>

Heap 종류로 Max-Heap, Min-Heap이 있다.<br>
+ Min Heap : 최소힙, 작은 값을 항상 위의 노드에 둔다.
+ Max Heap : 최대힙, 가장 큰 값을 맨 위에 둔다.

## Binary Heap
![힙과 이진트리](https://user-images.githubusercontent.com/57389368/223467579-739129c5-5a2f-4795-bafc-f4da1631a402.png) <br><br>

각 노드의 자식노드의 최대개수는 힙의 종류에 따라 다르지만, <br>
대부분의 경우는 자식노드의 개수가 최대 2개인 이진 힙(binary heap)을 사용한다.


+ 배열로 표현하기 좋은 구조
+ 배열에 트리의 값들을 넣어줄 때, 1번 인덱스부터 루트노드가 시작된다. (노드의 고유번호값과 배열의 인덱스를 일치시키기 위함)
+ Max Heap에서는 Root node 에 있는 값이 제일 크므로,  최대값을 찾는데 소요되는 연산의 time complexity 이 O(1)이다. 
+ Min heap 에서는 최소값을 찾는데 소요되는 연산의 time complexity 가 O(1)이다.

## Heapify 구현하기
배열(Array)로 표현된 이진 트리(Binary tree)의 자료구조를 갖는 힙(Heap)을 생성하는 과정이다.
>  Min Heap 혹은 Max Heap 조건을 만족해야 한다.
 
 <br>
 
 #### 특징
+ 맨 마지막 노드를 루트 노드로 대체시킨 후, 다시 heapify 과정을 거쳐 heap 구조를 유지한다. 
+ 이런 경우에는 결국 O(log n)의 시간복잡도로 최대값 또는 최소값에 접근할 수 있게 된다.

<br>

#### 배열(arrays)를 통해서 설계하는 방법
```
1. 배열의 0번째는 사용하지 않는다.
2. 부모는 arr[(i-1)/2]
3. 왼쪽 자식노드는 arr[2i+1]
4. 오른쪽 자식노드는 arr[2i+2]
```

### Max-Heap Heapify 구현하기
`1` build max-heap 
> 일반 배열을 힙으로 구성한다. <br>
> 그 과정에서 자식노드로부터 부모 노드를 비교한다. <br>
> n/2-1부터 0까지 인덱스를 돈다. <br>


 `2` extreact-max
> 루트노드를 제거하고 마지막 노드를 루트로 옮긴다.

 `3` 다시 1~2 과정을 반복한다.
 
  <br><br>
  
 > 예시
 ```
정렬할 요소
3 7 5 4 2 8 

정렬 후
8 7 5 4 2 3 
```

  <br><br>
 
 > 코드
 ```java
 package hackerrank;

class HeapSort {

    HeapSort(int[] a) {
        sort(a, a.length);
    }

    // 힙 구조로 구성한다. (Build Max-heap)
    private static void sort(int[] a, int size) {
        // 자식이 없는 경우 바로 return 해준다.
        if (size < 2) return;

        // 부모 인덱스는 가장 마지막 인덱스.
        int parentId = getParent(size-1);

        // max heap
        for (int i=parentId; i>=0; i--) {
            // 부모노드(인덱스 i)를 1씩 줄이면서 heap 조건을 만족시키도록 재구성한다.
            heapify(a, i, size-1);
        }

        for (int i=size-1; i>0; i--) {
            swap(a, 0, i-1); // root인 0번째 인덱스와 i번째 인덱스의 값을
            heapify(a, 0, i-1);// 0~(i-1)까지의 서브트리에 대해 min heap을 만족하도록 재구성한다.
        }
    }

    //  swap : 두 인덱스의 원소를 교환한다.
    private static void swap(int[] a, int index1, int index2) {
        int temp = a[index1];
        a[index1] = a[index2];
        a[index2] = temp;
    }

    // build max-heap
    private static void heapify(int[] a, int parentId, int lastId) {
        int leftChild_id;   // 왼쪽 자식노드
        int rightChild_id; //오른쪽 자식노드
        int largestId;    //  가장 큰 값

        // 현재 부모 인덱스의 자식노드 인덱스가 마지막 인덱스를 넘지 않을 때 까지 반복한다.
        while ((parentId*2)+1 <= lastId) {
            leftChild_id = (parentId*2)+1;
            rightChild_id = (parentId*2)+2;
            largestId = parentId; // 현재 부모 인덱스를 가장 큰 값을 같도록 한다.

            // 왼쪽 자식노드와 비교한다.
            // 현재 가장 큰 인덱스보다 왼쪽 자식노드가 큰 경우
            if (a[leftChild_id] > a[largestId]) {
                largestId = leftChild_id; // 가장 큰 인덱스를 왼쪽 자식노드로 바꾼다.
            }

            // 오른쪽 자식노드와 비교
            // 현재 가장 큰 인덱스보다 오른쪽 자식노드의 값이 더 큰 경우
           if (rightChild_id <= lastId && a[rightChild_id] > a[largestId]) {
                largestId = rightChild_id; // 가장 큰 인덱스는 오른쪽 자식노드가 된다.
            }

          // 가장 큰 인덱스와 부모노드가 같지 않다는 것
          // 위 자식노드 비교 과정에서 현재 부모노드보다 큰 노드가 존재한다는 뜻이다.
          // 그럴 경우, 해당 자식 노드와 부모노드를 교환해주고,
            // 교환된 자식노드를 부모노드로 삼은 서브트리를 검사하도록 재귀호출한다.
            if (largestId != parentId) {
                swap(a, parentId, largestId);
                parentId = largestId; // 교환이 된 자식노드를 부모 노드가 되도록 교체한다.
            } else {
                return;
            }
        }
    }

    // 부모 노드를 구한다.
    private static int getParent(int child) {
        return (child-1)/2;
    }
}
public class Main {
    public static void main(String[] args) {

        System.out.println("정렬할 요소");
        int[] a = {3,7,5,4,2,8}; // size = 6;
        for (int i : a) {
            System.out.print(i+" ");
        }
        System.out.println();

        HeapSort sort = new HeapSort(a);
        System.out.println("정렬 후");
        for (int i : a) {
            System.out.print(i +" ");
        }
    }

}
```


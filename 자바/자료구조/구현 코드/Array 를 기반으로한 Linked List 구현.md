```java
package hackerrank;

class LinkedList {
    private Node[] elementData = new Node[100];
    private Node head; // head 노드
    private Node tail;  // tail 노드
    private int size; //리스트 크기

    public LinkedList() {
        this.head = elementData[0]; 
        this.tail = elementData[size];
    }

    // 리스트에 들어갈 객체 Node
    private class Node {
        private Object data;
        public Node(Object input) {
            this.data = input;
        }

        public String toString() {
            return String.valueOf(this.data);
        }
    }

    // 첫번째 노드들 추가한다.
    void addFist(Object element) {
        Node newNode = new Node(element);
        add(0,newNode);

    }

    // 마지막 노드를 추가한다.
    // (마지막 노드의 index 크기가 size-1이였기 때문이다.)
    boolean addLast(Object element) {
        Node newNode = new Node(element); // 새로운 노드를 생성해서,
        elementData[size] = newNode; // 리스트 크기 index의 노드로 추가한다. 
        head = elementData[0]; // head 노드가 index 0인 노드임을 표시한다.
        size++; // 리스트 크기를 늘린다.
        tail = elementData[size-1]; // tail 노드를 지정한다.
        return true;
    }

    // index 번째 노드를 추가한다.
    boolean add(int index, Object element) {
       // element 중간에 데이터를 추가하기 위해서는, tail노드에서 해당 index의 노드까지 뒤로 한칸씩 이동시켜야 한다.
        for (int i=size-1; i>=index; i--) { 
            elementData[i+1] = elementData[i]; 
        }
        Node newNode = new Node(element); // 새로운 노드를 생성해서,
        elementData[index] = newNode; // 해당 index에 넣는다.
        head = elementData[0]; //  head 노드가 index 0인 노드임을 표시한다.
        size++; // 리스트 크기를 늘린다.
        tail = elementData[size-1]; // tail 노드를 지정한다.
        return true;
    }

    public String toString() {
        String str = "[";
        for (int i=0; i<size; i++) {
            str += elementData[i].toString();
            if (i<size-1) str+=",";
        }
        return str + "]";
    }

    // 해당 index 번째 노드를 삭제한다.
    Node remove(int index) {
        Node removed = elementData[index]; // 삭제할 노드를 removed 변수에 저장하고,

      // 삭제할 노드의 다음 노드부터 tail노드까지 순차적으로 이동해서 빈자리를 채운다.
        for (int i=index+1; i<=size-1; i++) { 
            elementData[i-1] = elementData[i];
        }
        head = elementData[0]; //  head 노드가 index 0인 노드임을 표시한다.
        size--; // 리스트 크기를 줄인다.
        tail = elementData[size-1]; // tail 노드를 지정한다.
        elementData[size] = null;  마지막 위치의 엘리먼트를 명시적으로 삭제해준다. 
        return removed;
    }

   // 첫번째 노드를 삭제한다.
    Node removeFirst() {
        return remove(0);
    }

  // 마지막 노드를 삭제한다.
    Node removeLast() {
        return remove(size-1);
    }

    int size() {
        return size;
    }

   // 해당 노드의 index를 찾는다.
    int indexOf(Object o) {
        Node node = new Node(o);
        int index = 0;
        for (Node element : elementData) { // head 노드부터 순회하면서 해당 노드가 있는지 찾는다. 
            if (element.toString().equals(node.toString())) return index;
            index++;
        }
        if (index == elementData.length) index = -1; // tail노드까지 찾아봐도 없다면 -1를 반환한다.
        return index;
    }

  // 확인차 head tail를 출력한다. 
    void printHead_Tail() {
        System.out.println("[head] " + head.toString() + ", [tail] " + tail.toString());
    }

}

public class Main {

    public static void main(String[] args) {
        LinkedList ll = new LinkedList();
        ll.addFist(10);
        ll.addFist(20);
        ll.addFist(30);
        ll.add(0,50);
        ll.addLast(40);
        
        System.out.println(ll.toString());
        ll.printHead_Tail();
        int index = ll.indexOf(40);
        System.out.println("indexOf(40) = " + index);
        
        ll.remove(3);
        ll.removeFirst();
        System.out.println(ll.toString());

    }

}
```

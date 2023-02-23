```java
import java.util.ArrayList;

class LinkedList {
    private static ArrayList<Node> elementData = new ArrayList<>();
    private static Node head; // head 노드
    private static Node tail; // tail 노드
    private static int size;  // 리스트 크기 

    public LinkedList() {
        this.size = elementData.size();
        if (size < 1) {  // 처음 list에 값이 없을 때 예외처리
            this.head = null;
            this.tail = null;
        } else {
            this.head = elementData.get(0);
            this.tail = elementData.get(size);
        }
    }

    // LinkedList 객체 단위 Node 
    private class Node {
        private Object data;
        public Node(Object input) {
            this.data = input;
        }
        public String toString() {
            return String.valueOf(this.data);
        }
    }

    // 0번째 노드를 추가한다.
    void addFist(Object element) {
        add(0, element);
    }

   // 마지막 노드를 추가한다. 
    boolean addLast(Object element) {
        Node newNode = new Node(element); // 새로운 노드를 생성하고,
        elementData.add(newNode); // 리스트에 추가한다.
        head = elementData.get(0);  // head는 리스트의 0번째 노드임을 표시한다.
        size++;  // 리스트 크기를 늘린다.
        tail = elementData.get(size-1); // tail은 리스트의 마지막 노드임을 표시한다.
        return true; 
    }

   //index번째 노드를 추가한다.
    boolean add(int index, Object element) {
        Node newNode = new Node(element); // 새로운 노드를 생성해서,
        elementData.add(index, newNode);  // 리스트에 해당 index에 추가한다.
        head = elementData.get(0);  // head는 리스트의 0번째 노드임을 표시한다.
        size++;  // 리스트 크기를 늘린다.
        tail = elementData.get(size-1); // tail은 리스트의 마지막 노드임을 표시한다.
        return true;
    }

    public String toString() {
        return elementData.toString();
    }

   //index 번째 노드를 삭제한다.
    Node remove(int index) {
        Node removed = elementData.get(index); // 삭제할 노드를 removed 변수로 지정하고,
        elementData.remove(removed); // 리스트에서 삭제한다.
        head = elementData.get(0); // head는 리스트의 0번째 노드임을 표시한다.
        size--; // 리스트 크기를 줄인다. 
        tail = elementData.get(size-1); // tail은 리스트의 마지막 노드임을 표시한다.
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
        Node node = new Node(o); //해당 노드를 
        int index = 0;
        for (Node element : elementData) { //리스트의 head 노드부터 순회하면서, 있는지 찾는다.
            if (element.toString().equals(node.toString())) return index;
            index++;
        }
        if (index == elementData.size()) index = -1; // 리스트의 끝 노드에 도착해도 못 찾았다면 -1를 반환한다.
        return index;
    }

   // head tail 노드를 확인하기 위해 만들었다. 
    void printHead_Tail() {
        System.out.println("[head] " + head.toString() + ", [tail] " + tail.toString());
    }

}

public class Main {

    public static void main(String[] args) {
        LinkedList ll = new LinkedList();
        ll.add(0,10);
        ll.add(0,20);
        ll.add(0,30);
        ll.add(0,50);
        ll.addLast(40);
        ll.addFist(60);
        System.out.println(ll.toString());

        System.out.print("print -> ");
        ll.printHead_Tail();

        ll.remove(3);
        ll.removeFirst();
        System.out.println(ll.toString());

        System.out.println("ll.indexOf(40) = " + ll.indexOf(40));
    }

}
```

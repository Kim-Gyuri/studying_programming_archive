

```java
class LinkedList {
    private Node head; //첫번째 노드를 가리키는 필드
    private Node tail; //마지막 노드를 가리키는 필드
    private int size = 0; // 리스트 크기

    // 리스트에 있는 객체 노드
    private class Node {
        private Object data; //데이터가 저장될 필드
        private Node next;   //이어진 다음 노드
        public Node(Object input) {
            this.data = input;
            this.next = null;
        }
        public String toString() {
            return String.valueOf(this.data);
        }
    }

    // head 노드로 추가한다.
    public void addFirst(Object input) {
        Node newNode = new Node(input); //새로운 노드를 생성해서,
        newNode.next = head; // head 앞에 붙인다.
        head = newNode; //이제 head 를 새로 생성한 노드로 지정한다.
        size++; // 리스트 크기를 늘린다.
        if (head.next == null) { //리스트에 노드가 1개 뿐인 경우,
            tail = head;         // head tail 는 같은 노드다.
        }
    }

    // 현재 tail 다음 노드를 추가한다.
    public void addLast(Object input) {
        Node newNode = new Node(input); //새로운 노드를 생성해서,
        if (size == 0) {  //리스트에 노드가 없는 경우 addFirst()로 처리된다.
            addFirst(input);
        } else {
            tail.next = newNode; // tail 다음 노드로 붙인다.
            tail = newNode;  // 이제 tail 노드는 새로운 노드로 바뀐다.
            size++; //리스트 크기를 늘린다.
        }
    }

    Node node(int index) {  //리스트 (index)번째 노드를 찾는다.
        Node x = head;
        for (int i=0; i<index; i++) {
            x = x.next;  //head 노드부터 index 까지 이동하면서 찾아간다.
        }
        return x;
    }

    // k번째 노드를 추가한다.
    public void add(int k, Object input) {
        if (k==0) { // 0번째 (첫번째 노드) 이므로 addFirst()로 head 노드로 추가한다.
            addFirst(input);
        } else {
            Node temp1 = node(k-1); // k번째 노드의 이전 노드를 temp1로 지정한다.
            Node temp2 = temp1.next;
            Node newNode = new Node(input); //새로운 노드를 생성해서
            temp1.next = newNode;  // k번째 노드로 지정한다.
            newNode.next = temp2;  // (새로운 노드의 다음 노드를 temp2로 지정해서 노드를 연결해준다.
            size++; //리스트 크기 늘린다.
            if (newNode.next == null) { // 만약 새로운 노드의 다음 노드가 없다면,
                tail = newNode;      //새로운 노드가 마지막 노드이기 때문에 tail 노드로 지정한다.
            }
        }
    }

    // 노드를 출력하기 위한 형태
    public String toString() {
        if (head == null) return "[]";  // 노드가 없다면 빈 []를 출력한다.

        // 탐색을 통해 전체 노드를 String 으로 리턴한다.
        Node temp = head; // head 노드부터
        String str = "[";
        while (temp.next != null) { // 끝 tail 노드까지 반복하여 포함시킨다.
            str += temp.data + ",";
            temp = temp.next;
        }
        str += temp.data;
        return str + "]";
    }

    // head 노드를 삭제한다.
    public Object removeFirst() {
        Node temp = head;
        head = temp.next; // 첫번째 노드의 다음 노드를 head 로 지정한다.

        Object returnData = temp.data; //// 데이터를 삭제하기 전에 리턴할 값을 임시 변수에 담는다.
        temp = null; // 삭제
        size--;
        return returnData;
    }

    // k번쨰 노드를 삭제한다.
    public Object remove(int k) {
        if (k == 0) return removeFirst(); //0번째 노드 이므로 removeFirst() 처리

        Node temp = node(k - 1); // k 번째 이전 노드를 temp로 지정한다.
        Node todoDeleted = temp.next; // 삭제할 k 번째 노드를 todo로 지정한다.
        temp.next = temp.next.next; //  temp - temp.next.next로 연결하여 k번째 노드를 끊는다.
        Object returnData = todoDeleted.data; 
        if (todoDeleted == tail) { // k번째 노드가 tail 이였다면 temp 노드가 tail 이 된다.
            tail = temp;
        }
        todoDeleted = null; //삭제
        size--; 
        return returnData;
    }

    // 마지막 노드를 삭제한다.
    public Object removeLast() {
        return remove(size-1); //index(size-1)를 삭제
    }

    public int size() {
        return size;
    }

    public Object get(int k) { 
        Node temp = node(k);
        return temp.data;
    }

    // 해당 노드의 index 를 구한다.
    public int indexOf(Object data) {
        Node temp = this.head; 
        int index = 0;

        while (temp.data != data) {  // head 노드부터 순차탐색한다.
            temp = temp.next;
            index++;
            if (temp == null) return -1;  //tail 노드까지 탐색했지만, 못 찾은 경우 -1를 반환
        }
        return index;
    }

}

public class Main {
    public static void main(String[] args) {
        LinkedList ll = new LinkedList();
        ll.addFirst(10);
        ll.addFirst(20);
        ll.addLast(30);
        ll.addLast(40);
        ll.add(1, 50);
        ll.removeFirst();
        System.out.println(ll.toString());
        System.out.println("ll.indexOf(50) = " + ll.indexOf(50));
    }
}
```

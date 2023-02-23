# DataStructure
+ (Array vs ArrayList vs LinkedList)[#Array-vs-ArrayList-vs-LinkedList]
    + Array
    + ArrayList
    + LinkedList
    + 자바로 ArrayList 구현
    + 자바로 LinkedList 구현
    + Array를 기반으로한 ArrayList 
    + Array를 기반으로한 LinkedList 구현
    + ArrayList를 기반으로한 LinkedList 구현

## Array vs ArrayList vs LinkedList

## Array를 이용하여 ArrayList를 구현하기

```java
class ArrayList {
    private int size;
    private Object[] elementData = new Object[100];
    public ArrayList() { }

    boolean addLast(Object element) {
        elementData[size] = element;
        size++;
        return true;
    }

    boolean add(int index, Object element) {
        for (int i=size-1; i>=index; i--) {
            elementData[i+1] = elementData[i];
        }
        elementData[index] = element;
        size++;
        return true;
    }

    boolean addFirst(Object element) {
        return add(0,element);
    }

    public String toString() {
        String str = "[";
        for (int i=0; i<size; i++) {
            str += elementData[i];
            if (i<size-1) str+=",";
        }
        return str + "]";
    }

    Object remove(int index) {
        Object removed = elementData[index];

        for (int i=index+1; i<=size-1; i++) {
            elementData[i-1] = elementData[i];
        }
        size--;
        elementData[size] = null;
        return removed;
    }

    Object removeFirst() {
        return remove(0);
    }

    Object removeLast() {
        return remove(size-1);
    }

    int size() {
        return size;
    }

    int indexOf(Object o) {
        for (int i=0; i<size; i++) {
            if (o.equals(elementData[i])) return i;
        }
        return -1;
    }

    ListIterator listIterator() {
        return new ListIterator();
    }

    class ListIterator {
        private int nextIndex = 0;

        public boolean hasNext() {
            return nextIndex < size;
        }

        public Object next() {
            return elementData[nextIndex++];
        }

        public boolean hasPrevious() {
            return nextIndex > 0;
        }

        public Object previous() {
            return elementData[--nextIndex];
        }

        public void add(Object element) {
            ArrayList.this.add(nextIndex++, element);
        }

        public void remove() {
            ArrayList.this.remove(nextIndex-1);
            nextIndex--;
        }
    }

}

public class Main {

    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        list.addLast(10);
        list.addLast(20);
        list.addFirst(30);
        System.out.println(list.indexOf(30));
        System.out.println(list.indexOf(10));

        list.removeLast();
        System.out.println(list.toString());
    }
}
```


## Array 를 기반으로한 Linked List 구현
```java
package hackerrank;

class LinkedList {
    private Node[] elementData = new Node[100];
    private Node head;
    private Node tail;
    private int size;

    public LinkedList() {
        this.head = elementData[0];
        this.tail = elementData[size];
    }

    private class Node {
        private Object data;
        public Node(Object input) {
            this.data = input;
        }

        public String toString() {
            return String.valueOf(this.data);
        }
    }

    void addFist(Object element) {
        Node newNode = new Node(element);
        add(0,newNode);

    }

    boolean addLast(Object element) {
        Node newNode = new Node(element);
        elementData[size] = newNode;
        head = elementData[0];
        size++;
        tail = elementData[size-1];
        return true;
    }


    boolean add(int index, Object element) {
        for (int i=size-1; i>=index; i--) {
            elementData[i+1] = elementData[i];
        }
        Node newNode = new Node(element);
        elementData[index] = newNode;
        head = elementData[0];
        size++;
        tail = elementData[size-1];
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

    Node remove(int index) {
        Node removed = elementData[index];

        for (int i=index+1; i<=size-1; i++) {
            elementData[i-1] = elementData[i];
        }
        head = elementData[0];
        size--;
        tail = elementData[size-1];
        elementData[size] = null;
        return removed;
    }

    Node removeFirst() {
        return remove(0);
    }

    Node removeLast() {
        return remove(size-1);
    }

    int size() {
        return size;
    }

    int indexOf(Object o) {
        Node node = new Node(o);
        int index = 0;
        for (Node element : elementData) {
            if (element.toString().equals(node.toString())) return index;
            index++;
        }
        if (index == elementData.length) index = -1;
        return index;
    }

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

## ArrayList를 기반으로한 Linked List 구현
```java
import java.util.ArrayList;

class LinkedList {
    private static ArrayList<Node> elementData = new ArrayList<>();
    private static Node head;
    private static Node tail;
    private static int size;

    public LinkedList() {
        this.size = elementData.size();
        if (size < 1) {
            this.head = null;
            this.tail = null;
        } else {
            this.head = elementData.get(0);
            this.tail = elementData.get(size);
        }
    }

    private class Node {
        private Object data;
        public Node(Object input) {
            this.data = input;
        }
        public String toString() {
            return String.valueOf(this.data);
        }
    }

    void addFist(Object element) {
        add(0, element);
    }

    boolean addLast(Object element) {
        Node newNode = new Node(element);
        elementData.add(newNode);
        head = elementData.get(0);
        size++;
        tail = elementData.get(size-1);
        return true;
    }

    boolean add(int index, Object element) {
        Node newNode = new Node(element);
        elementData.add(index, newNode);
        head = elementData.get(0);
        size++;
        tail = elementData.get(size-1);
        return true;
    }

    public String toString() {
        return elementData.toString();
    }

    Node remove(int index) {
        Node removed = elementData.get(index);
        elementData.remove(removed);
        head = elementData.get(0);
        size--;
        tail = elementData.get(size-1);
        return removed;
    }

    Node removeFirst() {
        return remove(0);
    }

    Node removeLast() {
        return remove(size-1);
    }

    int size() {
        return size;
    }

    int indexOf(Object o) {
        Node node = new Node(o);
        int index = 0;
        for (Node element : elementData) {
            if (element.toString().equals(node.toString())) return index;
            index++;
        }
        if (index == elementData.size()) index = -1;
        return index;
    }

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

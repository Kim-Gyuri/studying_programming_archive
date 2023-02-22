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
        private Node next;
        public Node(Object input) {
            this.data = input;
            this.next = null;
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
        size++;
        return true;
    }


    boolean add(int index, Object element) {
        for (int i=size-1; i>=index; i--) {
            elementData[i+1] = elementData[i];
        }
        Node newNode = new Node(element);
        elementData[index] = newNode;
        size++;
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
        size--;
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
        for (int i=0; i<size; i++) {
            if (o.equals(elementData[i])) return i;
        }
        return -1;
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

       ll.remove(3);
       ll.removeFirst();
       System.out.println(ll.toString());
    }

}
```


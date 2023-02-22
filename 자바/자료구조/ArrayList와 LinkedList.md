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

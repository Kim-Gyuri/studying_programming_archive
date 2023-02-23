```java
class ArrayList {
    private int size; // 현재 리스트 크기
    private Object[] elementData = new Object[100];
    public ArrayList() { }

    // 마지막 element를 추가한다. 
    boolean addLast(Object element) {
        elementData[size] = element; 
        size++; //리스트 크기를 늘린다.
        return true;
    }

    // index 번째 element 를 추가한다.
    boolean add(int index, Object element) {
        //element 중간에 데이터를 추가하기 위해서는
        // 마지막 element 부터 해당 index 의 element 까지 뒤로 한칸씩 이동시켜야 합니다.
        for (int i=size-1; i>=index; i--) {
            elementData[i+1] = elementData[i];
        }
        elementData[index] = element; // 해당 index element로 추가한다.
        size++;  // 현재 리스트 크기를 증가한다.
        return true;
    }

    // 0번째 element 추가한다.
    boolean addFirst(Object element) {
        return add(0,element);
    }

    // 리스트 전체 element 를 출력한다.
    public String toString() {
        String str = "[";
        for (int i=0; i<size; i++) {
            str += elementData[i];
            if (i<size-1) str+=",";
        }
        return str + "]";
    }

    // 해당 index 의 element 를 삭제한다.
    Object remove(int index) {
        Object removed = elementData[index]; // 삭제할 데이터를 removed 변수에 저장한다.

        //  삭제된 element 다음 element 부터 마지막 element 까지 순차적으로 이동해서 빈자리를 채운다.
        for (int i=index+1; i<=size-1; i++) {
            elementData[i-1] = elementData[i];
        }
        size--; // 리스트 크기를 줄인다.
        elementData[size] = null; //마지막 위치의 element 를 명시적으로 삭제해준다.
        return removed;
    }

    // 첫번째 element 삭제한다.
    Object removeFirst() {
        return remove(0);
    }

    // 마지막 element 삭제한다.
    Object removeLast() {
        return remove(size-1);
    }

    int size() {
        return size;
    }

    // 해당 element 의 index 를 구한다.
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
        System.out.println(list.toString());
        System.out.println("list.indexOf(30) = " + list.indexOf(30));
        System.out.println("list.indexOf(10) = " + list.indexOf(10));

        list.removeLast();
        System.out.println(list.toString());
    }
}
```

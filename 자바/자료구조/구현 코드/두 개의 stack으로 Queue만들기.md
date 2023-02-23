## Queue와 Stack 차이점
![두개의 stack으로 queue 만들기 _1](https://user-images.githubusercontent.com/57389368/187603759-f45dcb87-0cf4-4448-82e5-c91bb2e6b458.JPG) <br>

`Queue` <br>
먼저 들어온 노드가 먼저 나간다. <br> <br>
`Stack` <br>
맨 나중에 온 노드가 먼저 나간다.

## 풀이
+ add할 때는 newStack에 담는다.
+ remove, peek할 때는 oldStack에 담는다.

### 포인트
![두 개의 stack으로 queue 만들기 _2](https://user-images.githubusercontent.com/57389368/187604814-d092b602-7642-4974-adee-b34891e59ca9.JPG) <br>
> 이 경우, (1)이 먼저 꺼내진다. <br> Queue처럼 먼저 들어온 (1)이 먼저 나간다.

## 코드
```java
import java.util.Stack;

class FullStackException extends Exception {
    FullStackException() {
        super();
    }
}

class EmptyStackSetException extends Exception {
    EmptyStackSetException() {
        super();
    }
}

class MyQueue<T> {
    Stack<T> stackNewest, stackOldest;
    MyQueue() {
        stackNewest = new Stack<T>(); //new, old 스택을 멤버 변수로 선언한다.
        stackOldest = new Stack<T>();
    }

    public int size() { //현재 데이터가 얼마 만큼 쌓였는지 반환하는 함수
        return stackNewest.size() + stackNewest.size();
    }

    public void add(T value) { 
        stackNewest.push(value);
    }

    private void shiftStacks() { //shiftStack()함수는 old가 비어 있어야 한다.
        if (stackOldest.isEmpty()) {
            while (!stackNewest.isEmpty()) { //루프를 돌면서 new가 다 비어질 때까지 
                stackOldest.push(stackNewest.pop());  //new의 데이터를 old로 push()해야 한다.
            }
        }
    }

    public T peek() {
        shiftStacks(); //shift를 호출하고
        return stackOldest.pop(); //old 스택에서 찾는다.
    }

    public T remove() {
        shiftStacks();  //shift를 호출하고
        return stackOldest.pop(); //old 스택에서 pop한다.
    }
}

public class Main {
    public static void main(String[] args) {
        MyQueue<Integer> q = new MyQueue<Integer>();
        q.add(1);
        q.add(2);
        q.add(3);

        System.out.println(q.remove());
        System.out.println(q.remove());
        System.out.println(q.remove());
    }
}
```

### 출력
```
1
2
3
```

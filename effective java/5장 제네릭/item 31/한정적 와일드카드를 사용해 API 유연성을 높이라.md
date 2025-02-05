# 매개변수화 타입의 불공변
매개변수화 타입은 불공변(invariant)이다. <br>
서로 다른 타입 Type1과 Type2가 있을 때 `List<Type1>`은 `List<Type2>`의 하위 타입도 상위 타입도 아니다. <br>
> 즉, `List<String>`은 `List<Object>`의 하위 타입이 아니다라는 뜻이다.

<br>

매개변수화 타입은 리스코프 치환 원칙에 어긋난다. <br>
`List<String>`은 `List<Object>`가 하는 일을 제대로 수행하지 못하니 하위 타입이 될 수 없다. <br>
> `List<String>`은 String 문자열만 넣을 수 있다.

<br>

### 예시 : Stack의 pushAll()
Stack의 pushAll()을 작성하여 일련의 원소를 스택에 넣으려고 한다. <br>
Iterable src의 원소 타입이 스택의 원소 타입과 일치하면 잘 작동한다. <br>
```java
import java.util.*;

// Generic stack with bulk methods using wildcard types (Pages 139-41)
public class Stack<E> {
    private E[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    @SuppressWarnings("unchecked")
    public Stack() {
        elements = (E[]) new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(E e) {
        ensureCapacity();
        elements[size++] = e;
    }

    public E pop() {
        if (size==0)
            throw new EmptyStackException();
        E result = elements[--size];
        elements[size] = null; // Eliminate obsolete reference
        return result;
    }

    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }

    // pushAll staticfactory without wildcard type - deficient!
    public void pushAll(Iterable<E> src) {
        for (E e : src)
            push(e);
    }

    // Little program to exercise our generic Stack
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        stack.pushAll(numbers);

        for (Integer number : numbers) {
            System.out.println(stack.pop());
        }
    }
}
```
List<Integer>를 생성하여 같은 원소 타입을 stack에 넣었기 때문에 잘 작동했다. <br>
![image](https://github.com/user-attachments/assets/bb46a43e-c5a0-4cd3-971d-4adb19f53c45) <br><br><br>


#  한정적 와일드 카드 타입을 이용한 확장
때론 불공변 방식보다 유연한 무언가가 필요하다. <br>
자바에는, 한정적 와일드카드 타입이라는 매개변수화 타입을 지원한다. <br>


### 예시 : 타입 오류
하지만 `Stack<Number>`로 선언한 후 pushAll()을 호출하면 컴파일 오류가 발생한다. <br>
매개변수화 타입이 불공변이기 때문이다. <br>
![image](https://github.com/user-attachments/assets/a5995c59-5952-4f17-a5b1-2b1eb5877f01) <br><br><br>

#  한정적 와일드 카드 타입을 이용한 확장
매개변수에 와일드카드 타입을 적용하면, 컴파일된다. <br>
```java
import java.util.*;

// Generic stack with bulk methods using wildcard types (Pages 139-41)
public class Stack<E> {
    private E[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    @SuppressWarnings("unchecked")
    public Stack() {
        elements = (E[]) new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(E e) {
        ensureCapacity();
        elements[size++] = e;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public E pop() {
        if (size==0)
            throw new EmptyStackException();
        E result = elements[--size];
        elements[size] = null; // Eliminate obsolete reference
        return result;
    }

    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }

    public void pushAll(Iterable<? extends E> src) {
        for (E e : src)
            push(e);
    }

    public void popAll(Collection<? super E> dst) {
        while (!isEmpty())
            dst.add(pop());
    }

    // Little program to exercise our generic Stack
    public static void main(String[] args) {
        Stack<Number> numberStack = new Stack<>();
        Iterable<Integer> integers = Arrays.asList(3, 1, 4, 1, 5, 9);
        numberStack.pushAll(integers);

        Collection<Object> objects = new ArrayList<>();
        numberStack.popAll(objects);

        System.out.println(objects);

    }
}
```

![image](https://github.com/user-attachments/assets/b6400efd-948f-446a-aece-ba089db75954) <br><br>

### 와일드카드 타입
매개변수화 타입 T가 생산자면 `<? extends T>`를 사용하고, 소비자라면 `<? super T>`를 사용한다. <br>
```java
    // 생성자 
    public void pushAll(Iterable<? extends E> src) {
        for (E e : src)
            push(e);
    }

    // 소비자
    public void popAll(Collection<? super E> dst) {
        while (!isEmpty())
            dst.add(pop());
    }
```

#### PECS : producer-extends, consumer-super (= Get and Put Princlple)
PECS 공식은 와일드카드 타입을 사용하는 기본 원칙이다.


#  반환타입에서의 한정적 와일드 카드 타입
반환타입에는 한정적 와일드카드 타입을 사용하면 안된다. <br>
클라이언트 코드에서도 와일드카드 타입을 써야 하기 때문이다. <br>
```java
public static <E> Set<E> union(Set<E> s1, Set<E> s2)
```

<br>

### 반환타입에는 한정적 와일드카드 타입을 사용하면 안된다.
반환 타입은 명확해야 하는데, `? extends T`는 구체적인 타입을 알 수 없어서 컴파일 오류가 발생한다.
![image](https://github.com/user-attachments/assets/b6f1d9ea-8989-435e-bc8b-0111d4119004)

# 매개변수와 인수
#### 매개변수
메서드 선언에 정의한 변수.
```java
void add(int value) {...}
```

#### 인수
메서드 호출 시 넘기는 실제값.
```java
add(10)
```

#### 제네릭
```
타입 매개변수(type parameter) : Class Set<T> {...}
타입 인수(type argument) : Set<Integer> = ...
```


# 타입매개 변수가 하나이면 와일드카드로 대체하기
메서드 선언에 타입 매개변수가 한 번만 나오면 와일드카드로 대체하라. <br>
이때 비한정적 타입 매개변수라면 비한정적 와일드카드로 바꾸고, 한정적 타입 매개변수라면 한정적 와일드카드로 바꾸면 된다. <br>

### Swap 메서드 선언
와일드카드 타입의 실제 타입을 알려주는 메서드를 `private 도우미 메서드`로 따로 작성하여 활용하는 방법이다. <br>
실제 타입을 알아내려면 이 도우미 메서드는 제네릭 메서드여야 한다. <br>

```java
import java.util.*;

public class Swap {

    public static void swap(List<?> list, int i, int j) {
        swapHelper(list, i, j);
    }

    // Private helper method for wildcard capture
    private static <E> void swapHelper(List<E> list, int i, int j) {
        list.set(i, list.set(j, list.get(i)));
    }


    public static void main(String[] args) {
        // Swap the first and last argument and print the resulting list
        List<String> argList = Arrays.asList(args);
        swap(argList, 0, argList.size() - 1);
        System.out.println(argList);
    }
}
```

swapHelper() 메서드는 리스트가 `List<E>`임을 알고 있다. <br>
이 리스트에서 꺼낸 값의 타입은 항상 E이고, E 타입의 값이라면 이 리스트에 넣어도 안전함을 알고 있다. <br><br><br>

### 와일드 카드 타입
와일드 카드 타입을 사용하면, set, add와 같은 추가하는 메서드를 작성하지 못한다. <br>
![image](https://github.com/user-attachments/assets/b9a9b1ba-e415-4f45-889a-a569b844795a) <br>
이 오류는 Java의 와일드카드와 관련된 타입 안정성 문제에서 발생하는데, <br>
즉, `List<?>` 타입에 대해 set()을 호출할 때 컴파일러가 타입을 보장할 수 없어서 발생하는 오류다. <br>
> `List<?>는 null 이외의 원소를 추가할 수 없다.` <br>
> 이유는 와일드카드(?)가 나타내는 타입이 정확히 무엇인지 알 수 없기 때문이다. <br>
> 타입을 명확하게 지정하고, `List<T>와 같이 제네릭을 사용하면 값을 안전하게 추가할 수 있다.`

<br>

따라서 실제 타입으로 변환해주기 위해 제네릭의 타입 매개변수를 사용해야하는데 <br>
이때 와일드 카드 타입을 실제 타입으로 바꿔주는 private 도우미 메서드를 따로 작성한다. <br>
실제 타입을 알아내려면 이 도우미 메서드는 제네릭 메서드여야하기 때문이다.

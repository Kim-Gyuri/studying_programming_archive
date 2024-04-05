# 멤버 클래스는 되도록 static으로 만들라.
중첩 클래스에는 4가지가 있으며 각각의 쓰임이 다르다. <br>
경우에 따라 어떤 종류의 중첩 클래스를 사용해야 하는지 정리해보자. <br><br>

먼저 중첩 클래스에 대한 핵심 포인트를 알고 정리하자. <br>

#### 멤버 클래스
메서드 밖에서도 사용해야 하거나 메서드 안에 정의하기엔 너무 길 경우, 멤버 클래스로 만든다.

#### 비정적 멤버 클래스
멤버 클래스의 인스턴스 각각이 바깥 인스턴스를 참조한다면, 비정적 멤버 클래스로 만든다.

#### 정적 멤버 클래스
멤버 클래스에서 바깥 인스턴스에 접근할 일이 없다면, 무조건 static을 붙여서 정적 멤버 클래스로 만들자.

#### 익명 클래스
중첩 클래스가 한 메서드 안에서만 쓰이면서 그 인스턴스를 생성하는 지점이 단 한곳이고 
해당 타입으로 쓰기에 접한한 클래스나 인터페이스가 이미 있는 경우,
익명 클래스로 만든다.

#### 지역 클래스
지역변수를 선언할 수 있는 곳이면 어디서든 가능하다.


## 1. 중첩클래스란 (nested class)
다른 클래스 안에 정의된 클래스를 말한다. <br>
중첩 클래스의 종류는  다음과 같다.
+ 정적 멤버 클래스
+ (비정적) 멤버 클래스
+ 익명 클래스
+ 지역 클래스

정적 멤버 클래스를 제외한 나머지는 내부 클래스(inner class)라고 한다. 


## 2. 정적 멤버 클래스
일반 클래스와 다른점은 다음과 같다.
+ 정적 멤버 클래스는 다른 클래스 안에 선언된다.
+ 정적 멤버 클래스는 바깥 클래스의 private 멤버에도 접근할 수 있다.

일반 클래스와 같은점은 다음과 같다.
+ 정적 멤버 클래스는 다른 정적 멤버와 똑같은 접근 규칙을 적용받는다.

### 코드
아래와 같이, 정적 멤버 클래스의 인스턴스를 생성할 때는 외부 클래스의 인스턴스를 생성할 필요가 없다.
```java
package item24;

// 바깥 클래스
public class OuterClass {
    private static String message = "out line";

    public static void outerMethod() {
        System.out.println(message);
    }

    // 정적 멤버 클래스
    public static class StaticInnerClass {
        private static String innerMessage = "in line";

        public static void innerMethod() {
            System.out.println(innerMessage);
        }
    }
}
```

StaticInnerClass는 정적 멤버이므로, Main 클래스에서 OuterClass의 인스턴스를 
생성할 필요 없이 직접 메서드를 호출할 수 있다.
```java
package item24;

public class Main {
    public static void main(String[] args) {
        OuterClass.outerMethod();

        // 정적 멤버 클래스의 메소드 호출
        OuterClass.StaticInnerClass.innerMethod();
    }
}
```

<br>

### 예시 : enum 열거타입도 정적 멤버 클래스다.
Calculator.Operator.PLUS나 Calcultator.Operation.MINUS 같은 형태로 원하는 연산을 참조할 수 있다.
```java
public class Calculator {

 public enum Operator {
    PLUS, MINUS, MULTIPLE, SUBTRACT
 }
}
```

## 3. 비정적 멤버 클래스
정적 멤버 클래스와의 차이점은 다음과 같다.
+ 정적 멤버 클래스는 static이 붙어있다.  (비정적 멤버 클래스는 static이 없다.)
+ 비정적 멤버 클래스의 인스턴스는 바깥 클래스의 인스턴스와 암묵적으로 연결된다.
+ 그래서 비정적 멤버 클래스의 인스턴스 메서드에서 정규화된 this를 사용해 바깥 인스턴스의 메서드를 호출하거나 바깥 인스턴스의 참조를 호출할 수 있다.

따라서 개념상, 중첩 클래스의 인스턴스가 바깥 클래스의 인스턴스와 `독립적으로 존재`할 수 있다면
정적 멤버 클래스로 만들어야 한다.
왜냐하면, 비정적 멤버 클래스는 바깥 인스턴스 없이는 생성할 수 없기 때문이다.
> 정규화된 this : "클래스명.this", this 형태로 바깥 클래스의 이름을 명시하는 용법을 말한다.

### 예시 코드
```java
/**
 * 중첩 클래스 : NestedExample
 */
public class NestedExample {
    private String message;
    public NestedExample(String message) {
        this.message = message;
    }

    public String getMessage() {
        // 비정적 멤버 클래스의 인스턴스와 바깥 클래스의 인스턴스가 연결되는 부분
        NonStaticInnerClass nonStaticInner = new NonStaticInnerClass("send message");
        return nonStaticInner.getMessageWithOuter();
    }

    /**
     * 비정적 멤버 클래스 (내부 클래스에 해당한다.)
     */
    private class NonStaticInnerClass {
        private String nonMessage;

        public NonStaticInnerClass(String nonMessage) {
            this.nonMessage = nonMessage;
        }

        public String getMessageWithOuter() {
            //  정규화된 this를 이용해서 바깥 클래스의 인스턴스의 메서드를 호출한다.
            return nonMessage + NestedExample.this.getMessage();
        }
    }
}
```


바깥 인스턴스없이 비정적 멤버 클래스를 생성할 수 없다.
아래와 같이 비정적 멤버 클래스를 public 으로 바꾸고,
```java
/**
 * 중첩 클래스 : NestedExample
 */
public class NestedExample {
    private String message;
    public NestedExample(String message) {
        this.message = message;
    }

    public String getMessage() {
        // 비정적 멤버 클래스의 인스턴스와 바깥 클래스의 인스턴스가 연결되는 부분
        NonStaticInnerClass nonStaticInner = new NonStaticInnerClass("send message");
        return nonStaticInner.getMessageWithOuter();
    }

    /**
     * 비정적 멤버 클래스 (내부 클래스에 해당한다.)
     */
    public class NonStaticInnerClass {
        private String nonMessage;

        public NonStaticInnerClass(String nonMessage) {
            this.nonMessage = nonMessage;
        }

        public String getMessageWithOuter() {
            //  정규화된 this를 이용해서 바깥 클래스의 인스턴스의 메서드를 호출한다.
            return nonMessage + NestedExample.this.getMessage();
        }


    }
}
```

<br><br>

### 직접 비정적 멤버 클래스를 호출하지말자
직접 "바깥 인스턴스의 클래스.new MemberClass(args)"를 호출해 수동으로 만들기도 한다.
하지만, 비정적 멤버 클래스의 인스턴스 안에 만들어져 메모리 공간을 차지하며 생성시간도 더 걸린다.
```java
public class Main {
    public static void main(String[] args) {
        NestedExample nestedExample = new NestedExample("hello");
        NestedExample.NonStaticInnerClass nonStaticInnerClass = nestedExample.new NonStaticInnerClass("hi");
    }
}
```

<br>

### 비정적 멤버 클래스의 쓰임 : 어댑터 역할
+ 어떤 클래스의 인스턴스를 감싸 마치 다른 클래스의 인스턴스처럼 보이게 하는 뷰로 사용하는 것이다.
+ Map 인터페이스의 구현체는 아래와 같이 (keySet, entrySet, value 메서드가 반환하는) 자신의 컬렉션 뷰를 구현할 때 비정적 멤버 클래스를 사용한다.
+ 비슷하게 Set과 List 같은 다른 컬렉션 인터페이스 구현들도 자신의 반복자를 구현할 때 비정적 멤버 클래스를 주로 사용한다.

<br>

### HashMap의 KeySet 비정적 멤버 클래스
```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {
    ...
    public Set<K> keySet() {
        Set<K> ks = keySet;
        if (ks == null) {
            ks = new KeySet();
            keySet = ks;
        }
        return ks;
    }

     // KeySet 클래스는 HashMap의 key를 담는 AbstractSet을 구현한 비정적 멤버 클래스입니다.
    final class KeySet extends AbstractSet<K> {
        public final int size()                 { return size; }
        public final void clear()               { HashMap.this.clear(); }
        public final Iterator<K> iterator()     { return new KeyIterator(); }
        public final boolean contains(Object o) { return containsKey(o); }
        public final boolean remove(Object key) {
            return removeNode(hash(key), key, null, false, true) != null;
        }
        public final Spliterator<K> spliterator() {
            return new KeySpliterator<>(HashMap.this, 0, -1, 0, 0);
        }
        public final void forEach(Consumer<? super K> action) {
            Node<K,V>[] tab;
            if (action == null)
                throw new NullPointerException();
            if (size > 0 && (tab = table) != null) {
                int mc = modCount;
                for (Node<K,V> e : tab) {
                    for (; e != null; e = e.next)
                        action.accept(e.key);
                }
                if (modCount != mc)
                    throw new ConcurrentModificationException();
            }
        }
    }
}
```

<br>

### 멤버 클래스에서 	바깥 인스턴스에 접근할 일이 없다면 무조건 static을 붙여서 정적 멤버 클래스로 만들자.
static을 생략하면 바깥 인스턴스로의 숨은 외부 참조를 갖게 된다. <br>
참조를 저장하려면 시간과 공간이 소비된다. <br>
더 심각한 문제는 가비지 컬렉션이 바깥 클래스의 인스턴스를 수거하지 못하는 메모리 누수가 생길 수  있다는 점이다.
> [메모리 누수에 대해 Heap Dump 확인하기 - 관련 글 첨부](https://tecoble.techcourse.co.kr/post/2020-11-05-nested-class/)

<br>

### private 정적 멤버 클래스로 정의하는 것이 최선이다
바깥 클래스 객체의 컴포넌트를 표현하는데 주로 쓰인다. 그 중 Map 인스턴스를 생각해보자. 
Map을 구현하는 많은 클래스는 내부적으로 키-값 쌍을 보관하는 Entry 객체를 사용한다.
각 Entry는 특정 맵에 속할 것 같지만 Entry 객체의 메서드인 getKey, getValue, setValue는 맵 객체에 접근할 수 없다.
따라서 Entry를 비정적 메서드로 표현하는 것은 낭비다.


## 4. 익명 클래스
익명 클래스 특징은 다음과 같다.
+ 이름이 없다.
+ 바깥 클래스의 멤버도 아니다.
+ 쓰이는 시점에 선언과 동시에 인스턴스가 만들어진다.
+ 비정적인 문맥에서 사용될 때만 바깥 클래스의 인스턴스를 참조할 수 있다.
+ 상수 정적변수 (final static) 외에는 정적 변수를 가질 수 없다.

### 익명 클래스의 제약사항
제약사항은 다음과 같다.
+ 선언한 지점에서만 인스턴스를 만들 수 있다. (단발성 이벤트 처리)
+ instance 검사나 클래스의 이름이 필요한 작업은 수행할 수 없다.
+ 여러 인터페이스를 구현할 수 없다. 
+ 인터페이스를 구현하는 동시에 다른 클래스를 상속할 수 없다.
+ 익명 클래스를 사용하는 클라이언트는 그 익명 클래스가 상위 타입에서 상속한 멤버 외에는 호출할 수 없다.
+ 표현식 중간에 등장하므로 10줄이 넘어가면 가독성이 떨어진다.

<br>

### 예시
익명 클래스를 사용하는 예시는 다음과 같다.
+ 람다(자바 7) 등장 이전에는 작은 함수 객체나 처리 객체를 만드는데 익명 클래스를 주로 사용했다.
+ 정적 팩터리 메서드를 구현할 때 사용되기도 한다.

<br>

### 예시 코드
인터페이스를 구현할 때, 아래와 같이 필드 선언과 동시에 인스턴스가 만들어진다.
```java
package item24;

public interface RemoteControl {
    public void turnOn();
    public void turnOff();
}
```

```java
package item24;

public class Anonymous {
    public void operate() {
        RemoteControl control = new RemoteControl() {
            @Override
            public void turnOn() {
                System.out.println("turn on TV");
            }

            @Override
            public void turnOff() {
                System.out.println("turn off TV");
            }
        };
        
        control.turnOn();
    }
}
```


## 5. 지역 클래스
지역 클래스 특징은 다음과 같다.
+ 네 가지 중첩 클래스 중 가장 드물게 사용된다.
+ 지역변수를 선언할 수 있는 곳이면 어디든 선언할 수 있다.
+ 유효 범위도 지역변수와 같다.
+ 멤버 클래스처럼 이름이 있고 반복해서 사용할 수 있다.
+ 익명 클래스처럼 비정적 문맥에서 사용될 때만 바깥 인스턴스를 참조할 수 있다.
+ 익명 클래스처럼 정적 멤버는 가질 수 없으며 가독성을 위해 짧게 작성해야 한다.

### 예시 코드
외부 클래스 OuterClass의 메소드 안에서만 지역 클래스를 호출할 수 있다.
```java
package item24;

// 외부 클래스
class OuterClass {
    void method() {
  
        // 지역 클래스
        class LocalInner {
            int a = 5;
        }

        LocalInner inner = new LocalInner();
        System.out.println(inner.a);
    }
}

public class Main {
    public static void main(String[] args) {
        OuterClass out = new OuterClass();
        out.method();
    }
}

```

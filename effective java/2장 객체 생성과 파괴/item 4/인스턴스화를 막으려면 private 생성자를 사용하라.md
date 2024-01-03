# 인스턴스화를 막으려면 private 생성자를 사용하라.

### 정적 메소드와 정적 필드
정적 메소드와 정적 필드만을 담은 클래스를 사용할 때가 있다. 
> java.lang.Math와 java.util.Arrays처럼 기본 타입 값이나 배열 관련 메서드들을 모아놓을 수 있다.

> '중요한 것` 자바 8부터 정적 메서드를 인터페이스에 넣을 수 있다. <br>
>  java.util.Collections처럼 특정 인터페이스를 구현하는 객체를 생성해주는 정적 메서드(팩토리)를 모아놓을 수도 있다.

> 마지막으로, final 클래스와 관련한 메서드들을 모아놓을 때도 사용한다. <br>
final 클래스를 상속해서 하위 클래스에 메서드를 넣는 건 불가능하기 때문이다.


##  추상클래스로 만드는 것으로는 인스턴스화를 막을 수 없다.
상속을 받았을 때는 인스턴스화가 가능한 문제점이 있다. (그래서 private 생성자로 막으면 된다.)
> 추상클래스: 추상화, 상속받아 구현체를 만들 수 있다.

```java
package study.effectivejava.item4;

public abstract class Person {

   // private 생성자를 추가하여, 인스턴스화를 막는다.
    private Person() {
        // 꼭 AssertionsError를 던질 필요는 없지만,
        // 클래스 안에서 실수로라도 생성자를 호출하지 않도록 해준다.
        throw new AssertionError();
    }
    public abstract void print();
}
```

```java
// 컴파일 오류 메시지: There is no default constructor available in 'study.effectivejava.item4.Person
// -> 상속을 불가능하게 하는 효과가 있다.
package study.effectivejava.item4;

class Child extends Person {
    public void print() {
        System.out.println("example text");
    }
}
```

```java
package study.effectivejava.item4;

public class AbstractTest {

    public static void doTest() {
        Child child = new Child();
        child.print();
    }
}
```

```java
package study.effectivejava.item4;

public class Main {
    public static void main(String[] args) {
        AbstractTest.doTest();
    }
}
```

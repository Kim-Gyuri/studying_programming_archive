# 1. @Override를 사용했을 때 장점
@Override는 메서드 선언에만 달 수 있으며, 이 애너테이션이 달렸다는 것은 상위 타입의 메서드를 재정의했음을 뜻한다. <br>
### 버그 예방
오타나 잘못된 시그니처로 인한 오버라이딩 실패를 컴파일 오류로 잡아준다.
### 상위 클래스의 메서드를 재정의할 때
@Override는 상위 클래스(또는 인터페이스)의 메서드를 정확히 오버라이딩하고 있는지를 컴파일러가 검증하게 만든다.

# 2. @Override를 작성하지 않아도 되는 예외 경우
## 1. 구체 클래스에서 상위 클래스의 추상 메서드를 재정의할 때는 굳이 @Override를 달지 않아도 된다.
```java
abstract class Animal {
    abstract void sound();
}

class Dog extends Animal {
    // @Override 없어도 컴파일 됨
    public void sound() {
        System.out.println("멍멍");
    }
}
```
Dog 클래스는 Animal의 추상 메서드 sound()를 재정의한다. <br>
이때 @Override를 붙이지 않아도 컴파일러가 자동으로 인식한다. <br>
### 이유?
추상 메서드는 반드시 구현해야 하므로, 오버라이딩이 강제되어 있다. <br>
따라서 굳이 @Override가 없어도 JVM이나 컴파일러가 문제 삼지 않는다.


# 결론
+ 재정의한 모든 메서드에는 @Override를 의식적으로 붙이는 것이 좋다. <br> 이렇게 하면 실수했을 때 컴파일러가 즉시 알려준다.
+ 구체 클래스에서 추상 메서드를 구현할 때는 @Override가 필수는 아니다.

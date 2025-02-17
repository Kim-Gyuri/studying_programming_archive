## ordinal 메서드
모든 열거 타입은 해당 상수가 그 열거 타입에서 몇 번째 위치인지를 반환하는 ordinal이라는 메서드를 제공한다.
### ordinal 메서드 잘못 쓸 때
#### 1. 상수 선언 순서를 바꾸는 순간 오동작한다.
```java
public enum OrderStatus {
    PENDING,   
    DELIVERED, // 순서를 바꿈 (original: 2)
    SHIPPED,   // original: 1
    CANCELLED
}
```
만약 OrderStatus 열거형의 상수 선언 순서를 바꾸면, ordinal() 값이 변한다. <br><br>

#### 2. 이미 사용 중인 정수와 값이 같은 상수는 추가할 방법이 없다.
```java
public enum OrderStatus {
    PENDING,    // 0
    SHIPPED,    // 1
    DELIVERED,  // 2
    COMPLETED   // 3 (DELIVERED와 같은 값을 원해도 불가능)
}
```
ordinal()은 열거형 내부에서 자동으로 관리되기 때문에 동일한 정수 값을 가진 상수를 추가할 방법이 없다. <br><br>
#### 3. 중간에 값을 추가하려면, 쓰이지 않는 더미(dummy) 상수를 같이 추가해야만 한다.
```java
public enum OrderStatus {
    PENDING,    // 0
    SHIPPED,    // 1
    DUMMY,      // 2 (더미 상, 쓰이지 않는 수를 추가해야 한다.)
    DELIVERED,  // 3
    CANCELLED   // 4
}
```
값을 중간에 비워둘 수 없으므로, 쓰이지 않는 더미 상수를 같이 추가해야 한다.<br><br>

## 해결책
#### 열거 타입 상수에 연결된 값은 ordinal 메서드로 얻지 말고, 인스턴스 필드에 저장하자.
```java
package effectivejava.chapter6.item35;

public enum Ensemble {
    SOLO(1), DUET(2), TRIO(3), QUARTET(4), QUINTET(5),
    SEXTET(6), SEPTET(7), OCTET(8), DOUBLE_QUARTET(8),
    NONET(9), DECTET(10), TRIPLE_QUARTET(12);

    private final int numberOfMusicians;
    Ensemble(int size) { this.numberOfMusicians = size; }
    public int numberOfMusicians() { return numberOfMusicians; }
}
```

<br><br>

## Enum의 API 문서
Enum의 API 문서를 보면, "대부분 프로그래머는 이 메서드를 쓸 일이 없다" 라고 쓰여 있다. <br>
EnumSet과 EnumMap 같이 열거 타입 기반의 범용 자료구조에 쓸 목적으로 설계되었다고 한다. <br>
따라서 쓸 일 없다고 생각하면 된다.

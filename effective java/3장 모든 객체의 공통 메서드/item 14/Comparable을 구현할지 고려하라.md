# Comparable 인터페이스
`Comparable<T>`은 자바에서 객체 간의 자연 `순서`(natural ordering) 를 정의하기 위한 인터페이스이다. <br>
이 인터페이스를 구현한 클래스는 compareTo(T o) 메서드를 반드시 오버라이드해야 하며, 이를 통해 두 객체의 순서를 비교할 수 있다.
```java
public interface Comparable<T> {
    int compareTo(T o);
}
```

### 특징
#### Object의 메서드가 아님
compareTo는 Comparable 인터페이스의 메서드이므로, Object 클래스에는 존재하지 않는다.

#### 동치성 이상의 비교 가능
equals()는 "같다 / 다르다"만 판단하지만, compareTo()는 "작다 / 같다 / 크다"까지 판단한다.

#### 제네릭 인터페이스
타입 안정성을 제공하며, 잘못된 타입 비교를 방지할 수 있다.


## compareTo 메서드 규약
```java
a.compareTo(b)
```
+ 음수 반환 → a < b
+ 0 반환 → a == b (논리적으로 같음)
+ 양수 반환 → a > b

# Comparable 인터페이스를 구현할 때 지켜야 할 compareTo 규약
## 1. 반사성, 대칭성, 추이성을 충족해야 한다.
비교 연산은 다음의 수학적 성질을 반드시 만족해야 한다.

#### 대칭성 (Symmetry)
```java
sgn(x.compareTo(y)) == -sgn(y.compareTo(x))
```
즉, x > y이면 y < x, x == y이면 y == x, x < y이면 y > x여야 한다. <br>
예외: y.compareTo(x)가 예외를 던질 경우에만 x.compareTo(y)도 예외를 던져야 한다.

#### 추이성 (Transitivity)
```java
x > y && y > z ⇒ x > z
```
순서 관계는 일관되어야 한다.

#### 동치성 비교 일관성
```java
if (x.compareTo(y) == 0) then sgn(x.compareTo(z)) == sgn(y.compareTo(z))
```
x와 y가 같다면, 어떤 z와 비교하더라도 x, y는 동일한 방식으로 비교되어야 한다.

#### 🔁 equals와의 일관성
```java
(x.compareTo(y) == 0) == (x.equals(y))
```
즉, 정렬 기준상 같다면 논리적 동등성도 같아야 한다. <br>
필수는 아니지만, 지키는 것이 바람직하다. <br>
지키지 않을 경우, 명확하게 문서화해야 한다.

```java
// TreeSet은 compareTo로 판단하므로,
// equals와 일치하지 않으면 논리적으로 다른 객체가 무시될 수 있음
Set<MyClass> set = new TreeSet<>();
```
> TreeSet, TreeMap 같은 정렬된 컬렉션은 compareTo 기반 비교를 사용한다. <br>
> 이때 equals와 불일치하면 중복 데이터가 들어가거나, 비정상 동작이 발생할 수 있다.

## 2. 기존 클래스를 확장한 구체클래스에서 새로운 값 컴포넌트를 추가하면 compareTo 지킬 방법이 없다.
"부모 클래스의 비교 규약"과 "하위 클래스에서 추가된 필드" 간에 순서를 정의할 때 충돌이 생기는 문제가 있다. <br>
예시 코드로 살펴보자. <br>

#### 부모 클래스 Point
```java
public class Point implements Comparable<Point> {
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public int compareTo(Point p) {
        return Integer.compare(x, p.x);  // x 값 기준 정렬
    }

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof Point)) return false;
        Point p = (Point) o;
        return x == p.x && y == p.y;
    }
}
```

#### 하위 클래스: ColorPoint (색상 정보 추가)
```java
public class ColorPoint extends Point {
    String color;

    public ColorPoint(int x, int y, String color) {
        super(x, y);
        this.color = color;
    }

    // compareTo 확장하고 싶다? 어떻게?
}
```

#### Point는 x좌표만 보고 순서를 정한다.
compareTo는 오직 x만 사용해서 정렬한다. <br>
그런데 ColorPoint는 color도 포함해서 순서를 정하고 싶을 수 있다. <br>

#### 하위 클래스에서 확장을 하면 문제가 발생한다.
```java
@Override
public int compareTo(Point p) {
    int result = super.compareTo(p); // x 좌표 비교
    if (result != 0) return result;

    if (p instanceof ColorPoint cp) {
        return this.color.compareTo(cp.color);  // 색상까지 비교
    }

    return 0;  // 색상 없는 일반 Point는 같다고 본다?
}
```

`문제 발생`: 규약을 위반할 수 있다.

```java
ColorPoint cp1 = new ColorPoint(1, 2, "red")

ColorPoint cp2 = new ColorPoint(1, 2, "blue")

Point p = new Point(1, 2)
```

| 비교                   | 결과                         |
| -------------------- | -------------------------- |
| `cp1.compareTo(cp2)` | `"red" vs "blue"` → 예: > 0 |
| `cp1.compareTo(p)`   | `x 좌표 같음` → 결과: `0`        |
| `cp2.compareTo(p)`   | `x 좌표 같음` → 결과: `0`        |


#### 추이성을 위반한다.
+ cp1 > cp2
+ cp1 == p
+ cp2 == p
  +  그런데 cp1 > cp2 이면 cp1 != p 여야 하는데 동치 취급해버리면 논리 깨짐.


### 우회법
 compareTo 규약을 지키기 어려울 때, 다음과 같이 우회하자. 
+ 상속 대신 컴포지션 사용
+ "정렬 기준이 되는 뷰 객체"를 반환하는 메서드 제공

#### Point 클래스에서 equals, hashcode를 생략한다.
```java
public class Point implements Comparable<Point> {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public int compareTo(Point other) {
        return Integer.compare(this.x, other.x);
    }

    // equals, hashCode 생략 가능
}
```

#### ColorPoint 클래스: 상속하지 않고 컴포지션을 사용한다.
```java
public class ColorPoint {
    private final Point point;  // 핵심: Point를 필드로 가짐 (컴포지션)
    private final String color;

    public ColorPoint(int x, int y, String color) {
        this.point = new Point(x, y);
        this.color = color;
    }

    // 핵심 뷰 메서드
    public Point asPointView() {
        return point;
    }

    public String getColor() {
        return color;
    }
}
```

#### 사용 예시: Point 기준으로 정렬
```java
List<ColorPoint> colorPoints = List.of(
    new ColorPoint(3, 5, "red"),
    new ColorPoint(1, 2, "blue"),
    new ColorPoint(2, 4, "green")
);

// Point 뷰로 바꿔서 정렬
colorPoints.stream()
    .sorted(Comparator.comparing(cp -> cp.asPointView()))
    .forEach(cp -> System.out.println(cp.getColor()));
```
olorPoint 자체는 Comparable을 구현하지 않아도,  내부의 Point를 기준으로 정렬이 가능하게 된다. <br>
compareTo 규약 위반 없이, 구조도 깔끔하다.


# compareTo 메서드 작성 요령
### (1) Comparable은 타입을 인수로 받는 제네릭 인터페이스이므로 compareTo 메서드의 인수 타입은 컴파일타임에 정해진다.

<br>

### (2) compareTo 메서드는 각 필드가 동치인지를 비교하는 게 아니라 순서를 비교한다. 
### Comparable을 구현하지 않은 필드나 표준이 아닌 순서로 비교해야 한다면, Comparator를 사용한다.

<br>

### (3)  compareTo 메서드 구현시 관계연산자 <, > 사용하는 방식은 거추장 스럽고 오류를 유발한다.
```java
// ❌ 이런 식은 버그 유발 가능
return this.age - other.age;  // 오버플로 가능성

// ✅ 아래처럼 작성하자
return Integer.compare(this.age, other.age);
```

<br>

### (4) 비교자 생성 메서드(comparator construction method)와 팀을 꾸려 메서드 연쇄로 비교자를 생성한다.
Comparator의 정적 메서드들을 조합해, 필드를 하나씩 비교하는 비교자 체인을 만든다는 뜻이다. <br>
비교자 생성 메서드는 다음과 같다. <br>
| 메서드                            | 설명              |
| ------------------------------ | --------------- |
| `Comparator.comparing()`       | 일반 객체 비교        |
| `Comparator.comparingInt()`    | `int` 값 비교      |
| `Comparator.comparingLong()`   | `long` 값 비교     |
| `Comparator.comparingDouble()` | `double` 값 비교   |
| `thenComparing()`              | 2차, 3차 정렬 기준 추가 |


```java
public class Person implements Comparable<Person> {
    private String name;
    private int age;

    @Override
    public int compareTo(Person other) {
        return Comparator
            .comparingInt(Person::getAge)
            .thenComparing(Person::getName)
            .compare(this, other);
    }
}
```
`comparingInt`는 객체 참조를 int 타입 키에 매핑하는 키 추출 함수 key extractor function를  인수로 받아, <br>
그 키를 기준으로 `순서를 정하는 비교자를 반환하는 정적 메서드`다. <br>
이 람다에서 입력 인수의 타입을 명시해 주었다. (프로그램 컴파일을 도와준 것과 같다.)

<br><br>

### (5) Comparator는 수많은 보조 생성 메서드들로 중무장하고 있다.
long과 double용으로도 변형 메서드가 준비되어 있다. 

| 메서드                                     | 설명                     |
| --------------------------------------- | ---------------------- |
| `comparingInt(ToIntFunction)`           | int 값을 추출해 정렬          |
| `comparingLong(ToLongFunction)`         | long 값을 추출해 정렬         |
| `comparingDouble(ToDoubleFunction)`     | double 값을 추출해 정렬       |
| `thenComparing(Comparator)`             | 앞 비교 결과가 같을 경우 이어서 비교  |
| `thenComparingInt(ToIntFunction)`       | 보조 정렬 기준으로 int 키 사용    |
| `thenComparingLong(ToLongFunction)`     | 보조 정렬 기준으로 long 키 사용   |
| `thenComparingDouble(ToDoubleFunction)` | 보조 정렬 기준으로 double 키 사용 |


<br>

### (6)  값의 차를 이용한 compareTo, compare 메서드를 사용하지 말자.
#### 값을 직접 빼서 비교하지 말자.
```java
// 잘못된 방식
Comparator<SomeClass> bad = (a, b) -> a.getValue() - b.getValue();
```
정수 오버플로가 발생할 수 있다. <br>
> 예: Integer.MIN_VALUE - 1 → 오버플로

부동 소수점 오류가 발생할 수 있다. <br>
> 예: double에서 작은 값의 차이 손실 가능

<br>

#### 값의 차(a - b)로 비교하지 말고, Integer.compare(a, b) 또는 Comparator.comparingInt(...)를 사용해 오버플로와 부동소수점 오류를 방지하자.
다읨의 두 방식 중 하나를 사용하자.

#### 정적 compare 메서드를 활용한 비교자
```java
// 기본 타입 비교
int result1 = Integer.compare(a, b);     // 정수
int result2 = Double.compare(x, y);      // 실수
```

<br>

#### 비교자 생성 메서드를 활용한 비교자
```java
Comparator<Person> byAge = Comparator.comparingInt(Person::getAge);

Comparator<Person> byNameThenAge = Comparator
    .comparing(Person::getName)
    .thenComparingInt(Person::getAge);
```

# 결론
순서를 고려해야 하는 값 클래스를 작성한다면 꼭 Comparable 인터페이스를 구현하여, <br>
그 인스턴스들을 쉽게 정렬하고, 검색하고, 비교 기능을 제공하는 컬렉션과 어우러 지도록 해야 한다. <br>


또한, Comparable의 compareTo와 Comparator의 차이점 및 함께 쓰는 경우는 아래와 같다. 

| 항목    | `Comparable<T>`                                | `Comparator<T>`                                                 |
| ----- | ---------------------------------------------- | --------------------------------------------------------------- |
| 비교 기준 | **자연 순서(Natural Order)**                       | **사용자 정의 순서(Custom Order)**                                     |
| 구현 위치 | **클래스 내부에서 구현** (`compareTo`)                  | **외부에서 정의** (`compare`, `comparingInt`, ...)                    |
| 정렬 사용 | `Collections.sort(list)`<br>`Arrays.sort(arr)` | `Collections.sort(list, comparator)`<br>`list.sort(comparator)` |
| 메서드   | `int compareTo(T other)`                       | `int compare(T a, T b)`                                         |

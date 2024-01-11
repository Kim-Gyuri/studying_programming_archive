### Chapter 3 : 모든 객체의 공통 메서드
Object는 객체를 만들 수 있는 구체 클래스지만 기본적으로는 상속해서 사용하도록 설계되었다. <br>
Object에서 final이 아닌 메서드(equals, hashCode, toString, clone, finalize)는 모두 재정의(overring)를 염두에 두고
설계된 것이라 재정의 시 지켜야 하는 일반 규약이 명확히 정의되어 있다. <br>
그래서 Object를 상속하는 클래스, 즉 모든 클래스는 이 메서드들을 일반 규약에 맞게 재정의해야 한다. <br><br>

이번 장에서는 final이 아닌  Object 메서드들을 언제 어떻게 재정의해야 하는지를 다룬다. <br>
그 중 finalize 메서드는 item8에서 다뤘으니 더 이상 언급하지 않는다. <br>
Comparable.compareeTo의 경우 Object의 메서드는 아니지만 성격이 비슷하여 이번 장에서 함께 다룬다. <br>

# equals는 일반 규약을 지켜 재정의하라
### 포인트
+ 꼭 필요한 경우가 아니라면 재정의하지 말자.
+ 그래도 필요하다면 핵심필드를 빠짐 없이 비교하며 다섯 가지 규약을 지키자.
+ 어떤 필드를 먼저 비교하느냐가 equals의 성능을 좌우하기도 한다. <br> 최상의 성능을 바란다면 다를 가능성이 더 크거나 비교하는 비용이 더 싼 필드를 먼저 비교하자.
+ 객체의 논리적 상태와 관련 없는 필드는 비교하면 안 된다.
+ 핵심 필드로부터 파생되는 필드가 있다면 굳이 둘다 비교할 필요는 없다. 편한 쪽을 선택하자.
+ equals를 재정의할 땐 hashCode도 반드시 재정의하자(item 11) 
+ equals의 매개변수 입력을 Object가 아닌 타입으로는 선언하지 말자.


## equals를 재정의 하면 안되는 경우
equals는 재정의하기 쉬워 보이지만 곳곳에 함정이 도사리고 있어서 자칫하면 끔찍한 결과를 초래한다. <br>
문제를 회피하는 가장 쉬운 길은 아예 재정의하지 않는 것이다. <br>

### 1. 각 인스턴스가 본질적으로 고유할 때
값 표현하는 게 아니라 동작하는 개체를 표현하는 클래스가 여기 해당된다.
#### 예시 : Thread
```java
package item10;

import java.lang.ref.WeakReference;
import java.util.regex.Pattern;

/**
 * WeakClassKey 클래스에서
 * 객체의 동등성을 판단하기 위해 약조한 참조된 객체의 비교를 수행한다.
 */
class WeakClassKey {

    private WeakReference<Object> weakReference;

    public Object get() {
        // weakRerence의 get() 메서드를 사용하여 약한 참조된 객체를 반환한다.
        return weakReference.get();
    }

    @Override
    public boolean equals(Object obj) {
        // 1. 자기 자신과의 비교
        if (obj == this)
            return true;

        // 2. 클래스 확인: obj가 WeakClassKey 클래스의 인스턴스인지 확인한다.
        if (obj instanceof WeakClassKey) {
            // 3. 참조된 객체 비교: 현재 객체와 obj의 참조된 객체 referent를 비교한다.
            Object referent = get();

            // 4. 동등성 확인: 현재 객체와 obj가 모두 null이 아니고, 두 참조가 같은 객체를 가리키는지 확인한다.
            return (referent != null) && (referent == ((WeakClassKey) obj).get());
        } else {
            // 5. obj가 WeakClassKey 클래스의 인스턴스가 아닌 경우, 동등하지 않다.
            return false;
        }
    }
}
```

---
### 2. 인스턴스의 '논리적 동치성(logical equality)'을 검사할 일이 없다
#### 논리적 동치성 검사 예시
java.util.regex.Pattern의 인스턴스가 같은 정규 표현식을 나타내는 지 검사한다. <br>
논리적 동치성을 검사할 필요가 없다고 판단한다면, equals 메서드를 재정의할 필요가 없다. <br>

#### java.util.regex.Pattern 클래스의 equals 메서드가 어떻게 논리적 동치성을 검사하는지를 예시로 알아보자.
Pattern 인스턴스의 정규 표현식 문자열이 동일한지를 확인하고 있다.  <br>
만약 이러한 검사가 필요하지 않다면, 그대로 equals 메서드를 사용하여 참조 동등성만을 비교하면 된다. <br>
```java
public class Main {
    public static void main(String[] args) {
        Pattern pattern1 = Pattern.compile("abc");
        Pattern pattern2 = Pattern.compile("abc");

        // 논리적 동치성 검사
        boolean areEqual = arePatternsEquals(pattern1, pattern2);
        System.out.println("동치성 검사 결과: " + areEqual);
    }

    // 패턴 문자열이 동일한지 비교
    private static boolean arePatternsEquals(Pattern pattern1, Pattern pattern2) {
        return pattern1.pattern().equals(pattern2);
    }
}
```

---
### 3. 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
Set의 구현체는  AbstractSet이 구현한 equals를 상속받아 쓴다. <br>
그 이유는 서로 같은 Set인지 구분하기 위해서 사이즈, 내부 값들을 비교하면 되기 때문이다. <br><br>

List 구현체들은 AbstractList로부터, Map 구현체들은 AbstractMap으로부터 상속받아 그대로 쓴다. <br>
>  Java의 컬렉션 프레임워크에서는 AbstractList와 AbstractMap 클래스가 각각 List와 Map 인터페이스를 구현한 기본적인 구현을 제공한다.  <br>
이러한 추상 클래스들은 일반적인 메서드의 구현을 제공하여 구현체들이 특정한 동작을 갖도록 도와준다.

<br>

#### AbstractSet에서의 equals() 메서드가 HashSet와 TreeSet의 경우에 이상한 결과를 초래할 수 있다.
 AbstractSet의 equals() 메서드는 다음과 같이 구현되어 있다. <br>
```java
public boolean equals(Object o) {
  // 1. 자기 자신과의 비교한다.
    if (o == this) {
       // 비교 대상이 자신과 같으면 true를 반환하고 종료한다.
        return true;
    }

   // 2. 비교 대상이 Set 인스턴스인지 확인한다.
    if (!(o instanceof Set)) {
       //set 인스턴스가 아니라면, false를 반환하고 종료한다.
        return false;
    }


  // 3. 비교 대상을 Collection으로 캐스팅 (Set이  Collection을 확장하므로 가능하다.)  
    Collection<?> c = (Collection<?>) o;
   
 // 4. 크기 비교
    if (c.size() != size()) {
    // 크기가 다르면  false를 반환하고 종료한다.
        return false;
    }
    try {
       // 5. containsAll 메서드를 사용하여 내부 값 비교
       // 현재 Set 객체의 모든 값이 비교 대상 Set 객체에 포함되어 있는지를 확인한다.
        return containsAll(c);
    } catch (ClassCastException | NullPointerException unused) {
     // 6. 예외 발생 시 false 반환
     // 이러한 예외가 발생할 경우, 
     // 비교 대상 객체가 올바른 타입이 아니거나 null을 포함하고 있는 것으로 간주됩니다.
        return false;
    }
}
```

테스트 코드를 작성해보면, 다음과 같은 결과가 발생한다.
```java
void setTest() {
    Set<String> hash = new HashSet<>();
    Set<String> tree = new TreeSet<>();
    hash.add("Set");
    tree.add("Set");

    System.out.println(hash.equals(tree)) // true;
}
```
+ 해시셋(HashSet)과 트리셋(TreeSet)은 둘 다 Set 인터페이스를 구현하고 있으며, AbstractSet을 상속받아 equals() 메서드를 구현하고 있다. <br> AbstractSet에서는 equals() 메서드 내에서 containsAll() 메서드를 사용하여 내부의 모든 요소를 비교한다.
+ 따라서 hash와 tree의 equals() 메서드를 호출하면, 내부적으로는 containsAll()이 사용되어 두 Set의 요소를 비교한다. <br> 이 때, 요소 "Set"이 두 Set에 공통적으로 포함되어 있으므로 equals() 메서드의 결과로 true가 반환된다.

#### 문제가 발생하는 원인
+ 여기서 문제가 발생하는 이유는 containsAll() 메서드가 AbstractCollection 클래스에서 정의되었고, <br>이 메서드는 AbstractCollection에서 제공되는 반복자(iterator)를 사용하여 비교를 수행한다. 
+ 이때, TreeSet은 원소를 정렬된 순서로 저장하고, 반면에 HashSet은 Hash 순서로 저장한다.

<br>

따라서, containsAll() 메서드가 순서에 민감한 경우 <br>
예를 들어 TreeSet의 경우에는, 이 순서 차이 때문에  equals() 비교 시 문제가 발생할 수 있다. <br><br>

#### 해결책
+ HashSet과 TreeSet 모두 containsAll() 메서드가 내부적으로 정렬이나 Hash 순서와 관련없이 비교하도록 구현되어 있어야 한다.
+ 그렇게 하면 상속된 equals() 메서드가 예상대로 동작하게 된다.

---
### 4. 클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다.
equals가 실수로 호출되는 걸 막고 싶다면, 다음처럼 구현해두자. <br>
```java
@Override 
public boolean equals(Object o){
    throw new AssertionError(); // 호출 금지
}
```

## equals를 재정의 해야 하는 경우
```
equals : 논리적인 동치성을 확인하고 싶을 때 재정의해둔다.
```
> `예외` <br>
> Enum : Enum은 값 클래스이다, Enum 클래스에서는 주로 참조 동등성을 기반으로 한다.  그래서 equals 메서드를 명시적으로 재정의할 필요가 없다.

```java
enum Color {
    RED, GREEN, BLUE
}

public class EnumEqualsExample {
    public static void main(String[] args) {
        Color color1 = Color.RED;
        Color color2 = Color.RED;

        // Enum의 equals는 참조 동등성을 확인
       // equals 메서드는 color1과 color2가 동일한 Color 열거형 상수를 참조하므로 true를 반환한다.
        System.out.println(color1.equals(color2)); // true
    }
}
```
>  Enum 클래스에서 equals 메서드는 `각 상수끼리의 참조 동등성을 확인`한다. <br> 
> 즉, `두 Enum 상수가 같은 상수 객체를 참조하는 경우에만 equals가 true를 반환한다.`

## equals의 일반 규약
null이 아닌 모든 참조 값 x, y, z에 대해, <br><br>

#### 반사성:
x.equals(x)는 true이다.
#### 대칭성:
x.equals(y)는 true이다. <br>
그렇다면, y.equals(x)도 true다.
#### 추이성:
x.equals(y)는 true이다. <br>
그렇다면 y.equals(z)도 true이다. <br>
그렇다면 x.equals(z)도 true이다.
#### 일관성:
x.equals(y)를 반복해서 호출해도 값이 변하지 않는다.
#### null-아님:
x.equals(null)는 false다.

## 1. 반사성
자기 자신과 같아야 한다.

## 2. 대칭성
서로에 대한 동치 여부에 똑같이 답해야 한다.
### 잘못된 코드 - 대칭성 위배!

```java
package item10;

import java.util.ArrayList;
import java.util.List;
import java.util.Objects;

/**
 * CaseInsensitiveString 클래스는
 * 대소문자를 무시하고 문자열을 비교하는 equals() 메서드를 가지고 있다.
 */
public final class CaseInsensitiveString {
    private final String s;

    public CaseInsensitiveString(String s) {
        this.s = Objects.requireNonNull(s);
    }

    // 대칭성 위배!
    @Override
    public boolean equals(Object o) {
        // CaseInsensitiveString 객체 간의 비교
        if (o instanceof CaseInsensitiveString)
            return s.equalsIgnoreCase(
                    ((CaseInsensitiveString) o).s);

        // 한 방향으로만 작동한다. (String 객채와의 비교한다.)
        if (o instanceof String)
            return s.equalsIgnoreCase((String) o);

        // 다른 유형의 객체와는 동일하지 않다면 false를 반환한다.
        return false;
    }

    /**
     * 실행 ->
     * CaseInsensitiveString 객체를 리스트에 추가하고, 리스트에 문자열 s가 포함되어 있는지 확인한다.
     */
    public static void main(String[] args) {
        // CaseInsensitiveString 객체 생성
        CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
        String s = "polish";

        // CaseInsensitiveString 객체를 담는 리스트를 생성한다.
        List<CaseInsensitiveString> list = new ArrayList<>();
        list.add(cis);

        // 리스트에 문자열 s가 포함되어 있는지 확인한다.
        System.out.println(list.contains(s)); // 결과 : false
    }
}
```
#### list.contains(s)가 false를 반환하는 이유는 equals 메서드의 구현이 대칭성을 위반하고 있기 때문이다.
CaseInsensitiveString 객체는 String과 대소문자를 무시하고 비교한다. <br> 
그러나 String 객체는 CaseInsensitiveString 객체와 대소문자를 구분하여 비교한다. <br>
이로 인해 대칭성이 깨지게 되어, list.contains(s)가 false를 반환한다.

### 대칭성 위배 문제를 해결하기
문제를 해결하려면 CaseInsensitiveString의 equals를 String과도 연동하겠다는 허황된 꿈을 버려야 한다.
```java
@Override
public boolean equals(Object o) {
    // CaseInsensitiveString 객체인지 확인
    // -> o가 CaseInsensitiveString 객체인지 확인한다.
    return o instanceof CaseInsensitiveString &&
            // s를 대소문자 무시하고 비교한다.
            s.equalsIgnoreCase(((CaseInsensitiveString) o).s);
}
```

## 3. 추이성
먼저, 간단히 2차원에서의 점을 표현하는 클래스를 예로 들어보자. <br>

#### 색상
```java
package item10;

/**
 * 색상
 */
public enum Color {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```

#### 2차원의 점을 표현하는 클래스
```java
package item10;

/**
 * 2차원에서의 점을 표현하는 클래스
 */
public class Point {
    private final int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof Point))
            return false;
        Point p = (Point) o;
        return p.x == x && p.y == y;
    }
}
```

#### 클래스를 확장해서 점에 색상을 더해본다.
```java
package item10;

/**
 * Point 클래스를 확장해서 점에 색상을 더한다.
 */
public class ColorPoint extends Point {

    private final Color color;

    public ColorPoint(int x, int y, Color color) {
        super(x, y);
        this.color = color;
    }

    /**
     * ColorPoint 클래스에서 상속받은 equals 메서드를 재정의하고,
     * 좌표 비교에 추가로 색상 비교를 수행하여 두 객체의 동일성을 판단한다.
     */
    @Override
    public boolean equals(Object o) {
        // 1. 인자로 전달된 객체가 ColorPoint의 인스턴스인지 확인한다.
        if (!(o instanceof  ColorPoint))
            // 다른 객체 타입이라면 false를 반환한다.
            return false;
        // 2. 부모 클래스의 equals 메서드 호출 및 색상을 비교한다.
        // 부모 클래스의 equals 메서드를 호출하여 좌표 비교를 수행하고,
        // 추가로 ColorPoint의 color 필드를 비교하여 두 객체가 동일한지 확인한다.
        return super.equals(o) && ((ColorPoint) o).color == color;
    }

    public static void main(String[] args) {
        Point p = new Point(1, 2);
        ColorPoint cp = new ColorPoint(1, 2, Color.RED);
        System.out.println(p.equals(cp) + " " + cp.equals(p));
    }
}
```

#### 분석
 코드의 출력은 true false가 됩니다.
+ p.equals(cp): Point 클래스의 equals 메서드는 좌표를 비교하므로, p의 좌표 (1, 2)와 cp의 좌표 (1, 2)가 동일하다. <br> 따라서 이 비교는 true가 되어 첫 번째 출력은 true가 된다.
+ cp.equals(p): ColorPoint 클래스의 equals 메서드는 Point의 equals 메서드를 호출하고, 추가로 색상 비교를 수행한다. <br> 두 객체의 좌표는 동일하지만, cp의 색상이 Color.RED로 설정되어 있고, p는 색상 정보가 없으므로 색상 비교는 false가 된다.  <br> 따라서 두 번째 출력은 false가 된다.

<br>

---
### 잘못된 코드 - 추이성 위배!
```java
    @Override public boolean equals(Object o) {
        if (!(o instanceof Point))
            return false;

        // o가 일반 Point면 색상을 무시하고 비교한다.
        if (!(o instanceof ColorPoint))
            return o.equals(this);

        // o가 ColorPoint면 색상까지 비교한다.
        return super.equals(o) && ((ColorPoint) o).color == color;
    }
```

```java
public static void main(String[] args) {
        ColorPoint p1 = new ColorPoint(1, 2, Color.RED);
        Point p2 = new Point(1, 2);
        ColorPoint p3 = new ColorPoint(1, 2, Color.BLUE);
        System.out.println(p2.equals(p3)); // true (색상을 무시해서 true)
        System.out.println(p1.equals(p3)); // false (색상까지 고려해서 false)
    }
```

<br>

---
### 잘못된 코드 - 무한 재귀 
Point의 또 다른 하위 클래스로 SmellPoint를 만들고, equals는 같은 방식으로 구현했다고 해보자. <br>
그런 다음 myColorPoint.equals(mySmellPoint)를 호출하면 stackOverflowError를 일으킨다.
#### Smell
```java
package item10;

/**
 * 냄새
 */
public enum Smell {
    SWEET, BAD, REFRESHED
}
```

#### Point의 또 다른 하위 클래스로 SmellPoint 만들기
```java
package item10;

public class SmellPoint extends Point {
    private final Smell smell;

    public SmellPoint(int x, int y, Smell smell) {
        super(x, y);
        this.smell = smell;
    }

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof Point)) {
            return false;
        }

        // o가 일반 Point인 경우, 색상을 무시하고 x,y 정보만 비교한다.
        if (!(o instanceof SmellPoint)) {
            return o.equals(this);
        }

        // o가 ColorPoint인 경우, 색상까지 비교한다.
        return super.equals(o) && this.smell == ((SmellPoint) o).smell;
    }

    public static void main(String[] args) {
        Point myColorPoint = new ColorPoint(2,3,Color.RED);
        Point mySmellPoint = new SmellPoint(2,3,Smell.SWEET);

        System.out.println(myColorPoint.equals(mySmellPoint));
    }
}
```
 myColorPoint.equals(mySmellPoint)를 호출하면 stackOverflowError를 일으킨다. <br>
![item10에서 stackOverflowError](https://github.com/Kim-Gyuri/studying_programming_archive/assets/57389368/1d8fac4c-3250-46a2-9d2b-96520da8c278)

---
### 잘못된 코드 - 리스코프 치환 원칙 위배
만약 추이성을 지키기 위해서 Point의 equals를 각 클래스들을 getClass를 통해서 같은 구체 클래스일 경우에만 비교하도록 하면 어떨까?
```java
@Override
public boolean equals(Object o) {
    // getClass
    if (o == null || o.getClass() != this.getClass()) {
        return false;
    }

    Point p = (Point) o;
    return this.x == p.x && this.y = p.y;
}
```

#### 이렇게 되면 동작은 하지만, 리스코프 치환원칙을 지키지 못하게 된다.
Point의 하위클래스는 정의상 여전히 Point이기 때문에 어디서든 Point로 활용되어야한다.

> `리스코프 치환 원칙`
> 1. 기반 클래스의 인스턴스는 언제나 해당 클래스의 서브 클래스의 인스턴스로 대체될 수 있어야 한다. <br>
> 2. 즉, 부모 클래스 타입의 객체를 사용하는 코드는 자식 클래스 타입의 객체를 사용해도 문제가 없어야 한다. <br>
> 3. 리스코프 치환 원칙을 위반하지 않으려면, 서브 클래스는 최소한 기반 클래스의 행위를 지켜야 하며, 서브 클래스의 특화된 동작은 기반 클래스의 행위를 침해해서는 안된다. <br>
> <br>
> 이런 경우에는 리스코프 치환 원칙이 위배될 수 있다. <br>
> 부모 클래스가 제공하는 메서드의 반환 타입이 부모 클래스 자체로 선언되어 있으면, 자식 클래스에서는 더 구체적인 타입으로 대체할 수 없다.  

---
### 우희 방법1. 상속 대신 컴포지션을 사용하자.
+ ColorPoint2가 Point 클래스를 포함하고 있으며, asPoint 메서드를 통해 내부적인 Point 객체를 반환하고 있다.  
+ 이는 컴포지션을 통해 Point의 기능을 활용하고 있음을 보여준다.

```java
package item10;

import java.util.Objects;

/**
 * 1. Point를 상속하는 대신 Point를 ColorPoint의 private 필드로 둔다.
 */
public class ColorPoint2 {
    private Point point;
    private Color color;

    public ColorPoint2(int x, int y, Color color) {
        this.point = new Point(x, y);
        this.color = Objects.requireNonNull(color);
    }

    /**
     * 이 ColorPoint의 point 뷰를 반환한다.
     */
    public Point asPoint() {
        return this.point;
    }

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof ColorPoint2)) {
            return false;
        }
        ColorPoint2 cp = (ColorPoint2) o;
        return this.point.equals(cp) && this.color.equals(cp.color);
    }
}
```

---
### 우회 방법2. 추상클래스의 하위클래스 사용하기
+ 추상클래스의 하위클래스에서는 equals 규약을 지키면서도 값을 추가할 수 있다.
+ 상위 클래스를 직접 인스턴스로 만드는게 불가능하기 때문에 하위클래스끼리의 비교가 가능해진다.

## 4. 일관성
```java
@Test
void consistencyTest() throws MalformedURLException {
    URL url1 = new URL("www.xxx.com");
    URL url2 = new URL("www.xxx.com");

    System.out.println(url1.equals(url2)); // 항상 같지 않다.
}
```
+ java.net.URL 클래스는 URL과 매핑된 host의 IP주소를 이용해 비교한다. <br> 같은 도메인 주소라도 나오는 IP정보가 달라질 수 있기 때문에 반복적으로 호출할 경우 결과가 달라질 수 있다.
+ 따라서 이런 문제를 피하려면 equals는 항시 메모리에 존재하는 객체만을 사용한 결정적 계산을 수행해야 한다.

## 5. null-아님
+ o.equals(null) == true인 경우는 상상하기 어렵지만, 실수로 NullPointerException을 던지는 코드 또한 허용하지 않는다.
+ 이러한 Exception을 막기 위해서 여러가지 방법이 존재하는데, 그중 하나는 null인지 확인을 하고 false를 반환하는 것이다. 하지만 책에서는 다른 방법을 추천하고 있다.

#### 명시적 null 검사가 필요없다.
```java
@Override 
public boolean equals(Object o) { 
    if (o == null) {
        return false;
    }
```

#### null 검사를 할 필요 없이 instanceof를 이용하자.
```java
@Override 
public boolean equals(Object o) {
    
    // 책에서 추천하는 내용은 null 검사를 할 필요 없이 instanceof를 이용하라는 것!
    // instanceof는 두번째 피연산자(Point)와 무관하게 첫번째 피연산자(o)거 null이면 false를 반환하기 때문이다. 
    if (!(o instanceof MyType)) {
        return false;
    }
    MyType mt = (MyType) o;
    ...
}
```

## 양질의 equals 메서드를 구현하는 4단계
#### 1. == 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.
#### 2. instanceof 연산자로 입력이 올바른 타입인지 확인한다.
#### 3. 입력을 올바른 타입으로 형변환한다.
#### 4. 입력 객체와 자기 자신의 대응되는 '핵심' 필드들이 모두 일치하는지 하나씩 검사한다.

```java
@Override
public boolean equals(final Object o) {
    // 단계 (1) == 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.
    if (this == o) {
        return true;
    }

    // 단계 (2)  instanceof 연산자로 입력이 올바른 타입인지 확인한다.
    if (!(o instanceof Point)) {
        return false;
    }

    // 단계 (3) 입력을 올바른 타입으로 형변환 한다.
    final Point p = (Point) o;

    // 단계 (4) 입력 개체와 자기 자신의 대응되는 핵심 필드들이 모두 일치하는지 하나씩 검사한다.
    
    // float와 double을 제외한 기본 타입 필드는 ==를 사용한다.
    return this.x == p.x && this.y == p.y;
    
    // 필드가 참조 차입이라면 equals를 사용한다.
    return str1.equals(str2);
    
    // null 값을 정상 값으로 취급한다면 Objects.equals로 NullPointException을 예방하자.
    return Objects.equals(Object, Object);

    // 모든 필드가 일치하면 true 반환
    return true;
}
```

## Equals 구현할 때 주의할 추가사항
> 위의 equals 메서드 구현 방법 4단계 이후 코드를 참고하면서

### 기본타입과 참조타입
+ 기본 타입: == 연산자 비교
+ 참조 타입: equals() 비교
+ 배열 필드: 원소 각각을 지침대로 비교한다. <br> 모두가 핵심 필드라면 Arrays.equals()를 사용한다.

> `float, double 필드` <br>
> Float.compare(float, float), Double.compare(double, double) 비교 (부동 소수 값을 다룬다.) <br>
> Float.equals(float)나 Double.equals(double)은 오토 박싱을 수반할 수 있어 성능상 좋지 않다. 

---
### null 정상값 취급 방지
Object.equals(object, object) 로 비교하여 NullPointException 발생을 예방하자.

---
### 필드의 표준형을 저장하자.
비교하기 복잡한 필드는 필드의 표준형(canonical form)을 저장한후 표준형끼리 비교하면 경제적이다.
> 이 기법은 특히 불변 클래스(item 17)에 제격이다. <br>
> 가변 객체라면 값이 바뀔 때마다 표준형을 최신 상태로 갱신해줘야 한다.

---
### 필드 비교 순서는 equals 성능을 좌우한다.
다를 가능성이 높은 필드를 먼저 비교하자. <br>
비교 비용이 싼 필드를 먼저 비교하자. <br><br>
핵심필드로부터 계산해낼 수 있는 파생 필드를 비교할 필요는 없다. <br>
하지만, 파생필드를 비교하는 쪽이 더 빠를 때도 있다.

---
### equals 재정의할 때는 hashcode도 반드시 재정의하자.
(item 11)

---
### 너무 복잡하게 해결하려 들지 말자.
> 일반적으로 별칭(alias)은 비교하지 않는게 좋다. 

> File 클래스라면, 심볼릭 링크를 비교해 같은 파일을 가리키는지를 확인하려 들면 안된다.

---
### Object 외의 타입을 매개변수로 받는 equals 메서드는 선언하지 말자.
이 메서드는 재정의한 게 아니다. 입력 타입이 Object가 아니므로 다중정의(item 52)한 것이다.
```java
// 잘못된 예 - 입력 타입은 반드시 Object여야 한다.
public boolean equals(MyClass o) {...}
```

---
### AutoValue 프레임워크
equals(hashcode도 마찬가지)를 작성하고 테스트하는 일은 지루하고 이를 테스트하는 코드도 항상 뻔하다. <br>
이 작업을 대신해줄 오픈소스로 AutoValue 프레임워크가 있다.
> Lombok이랑 비슷한 느낌의 라이브러리다.

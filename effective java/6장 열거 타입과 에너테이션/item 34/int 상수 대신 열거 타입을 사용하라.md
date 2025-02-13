### 열거 타입(Enum)
일정 개수의 상수 값을 정의한 다음, 그 외의 값은 허용하지 않는 타입이다. <br>
 자바에서 서로 연관있는 상수를 편리하게 관리하기 위해 사용한다. 요일, 순위, 성적 등을 나타내기 위해 사용한다. <br>
```java
public enum Week {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
}
```

# 1. 정수 열거 패턴 (int enum pattern)
자바에서 열거 타입을 지원하기 전에는 다음 코드처럼 정수 상수를 한 묶음 선언해서 사용하곤 했다. <br>
```java
public static final int APPLE_FUJI = 0;
public static final int APPLE_PIPPIN = 1;

public static final int ORANGE_NAVEL = 0;
public static final int ORANGE_TEMPLE = 1;
public static final int ORANGE_BLOOD = 2;
```

## 정수 열거 패턴의 단점
하지만, 정수 열거 패턴에는 단점이 많다. <br>
### 1. 타입 안전을 보장할 방법이 없으며 표현력도 좋지 않다.
오렌지를 건네야 할 메서드에 사과를 보내고 동등 연산자(==)로 비교하더라도 컴파일러는 아무런 경고 메시지를 출력하지 않는다. <br>
![image](https://github.com/user-attachments/assets/460685f7-4bd0-4e05-982e-113c13d1cfc8) <br><br>
정수 상수(static final int)로 과일 종류를 구분할 경우, 사과용 상수를 오렌지용 메서드에 전달해도 컴파일 에러가 발생하지 않는다. <br>
이유는 int 타입이기 때문에 타입 안전성이 보장되지 않기 때문이다. <br><br><br>
### 2. 접두어를 써서 이름 충돌을 방지해야 한다.
접두어를 사용하면 같은 이름이더라도 다른 의미를 가진 상수끼리 충돌하는 것을 방지할 수 있다. <br>
```java
    // 수은(원소)
    public static final int ELEMENT_MERCURY = 80;
    // 수성(행성)
    public static final int PLANET_MERCURY = 1;
```

<br><br>

### 3. 평범한 상수를 나열한 것뿐이라 컴파일하면 그 값이 클라이언트 파일에 그대로 새겨진다.
Java에서 static final int 같은 기본형 상수는 컴파일 시점에 해당 값을 직접 코드에 삽입한다. <br>
즉, 상수가 정의된 클래스가 변경되어도 클라이언트 코드(Client Code, 상수를 사용하는 코드)는 자동으로 업데이트되지 않는다. <br>
따라서, 상수의 값이 바뀌면 클라이언트도 반드시 다시 컴파일해야 한다. <br><br>
예시 코드를 작성했다. <br>
```java
// 상수 정의 파일
public class Constants {
    public static final int MAX_USERS = 100;  // 최대 사용자 수
}
```
```java
// 상수를 사용하는 파일
public class Client {
    public static void main(String[] args) {
        System.out.println("최대 사용자 수: " + Constants.MAX_USERS);
    }
}
```
Client.java를 컴파일하면 Constants.MAX_USERS의 값 100이 그대로 코드에 삽입된다. <br>
이후 Constants.java에서 MAX_USERS = 200;으로 변경해도 Client.java를 다시 컴파일하지 않으면 값이 여전히 100으로 남아 있는다. <br><br><br>

### 4. 같은 정수 열거 그룹에 속한 모든 상수를 한 바퀴 순회하는 방법도 마땅치 않다.
정수를 이용해 열거형 그룹을 만들면, 그룹에 속한 모든 값을 한 번에 순회하기 어렵다. <br>
Java의 static final int 방식은 배열이나 리스트에 따로 담아두지 않는 이상, 모든 값을 반복(iterate)하는 방법이 없다. <br><br>
순회하려면 배열을 직접 만들어야 한다. <br>
아래 코드처럼, APPLES, ORANGES 배열을 직접 만들어야 한다. <br>
![image](https://github.com/user-attachments/assets/172b63d0-514e-4122-bf36-a954fd70c2de)

# 2. 문자열 열거 패턴(String enum pattern)
문자열 열거 패턴이란, 정수 대신 문자열 상수를 사용하는 변형 패턴이다. <br>
## 문자열 열거 패턴의 단점
문자열 열거 패턴은 더 나쁘다. <br>
#### 1. 문자열에 오타가 있어도 컴파일러는 확인할 길이 없으니 자연스럽게 런타인 버그가 생긴다.
#### 2. 문자열 비교에 따른 성능 저하 역시 당연한 결과다.

<br> 문자열 오타로 인한 런타임 버그 발생하는데, 예시 코드와 함께 보자. <br>
![image](https://github.com/user-attachments/assets/d535fca3-101f-4df5-97b0-bdd9dfd6f54b) <br>
![image](https://github.com/user-attachments/assets/c6676ecb-343e-4dbd-98ed-5a040da2f70f) <br>
"APPLE_FUJ"처럼 오타가 있어도 컴파일 오류 없이 실행된다. <br>
이로 인해 예상치 못한 버그가 발생할 가능성이 높다. <br><br><br>

# 열거 타입(enum type)
자바는 열거 패턴의 단점을 말끔히 씻어주는 동시에 여러 장점을 안겨주는 대안을 제시했다. <br>
바로 열거 타입이다. 다음은 열거 타입의 가장 단순한 형태다. <br>
```java
public enum Apple {FUJI, PIPPIN, GRANNY_SMITH}
public enum Orange {NAVEL, TEMPLE, BLOOD}
```
자바의 열거 타입은 완전한 형태의 클래스다. (단순한 정숫값일 뿐인) 다른 언어의 열거 타입보다 강력하다. <br>

### 자바 열거 타입을 뒷받침하는 아이디어
#### 1. 열거 타입 자체는 클래스다.
#### 2. 상수 하나당 자신의 인스턴스를 하나씩 만들어 public static final 필드로 공개한다.
자바에서 enum을 선언하면, 컴파일러가 자동으로 클래스를 생성한다. <br>
![image](https://github.com/user-attachments/assets/ad6939c1-e65d-41d3-9381-9578e9773fd1) <br>
위 코드는 내부적으로 다음과 같은 클래스처럼 동작한다.
```java
public final class Fruit extends Enum<Fruit> {
    public static final Fruit APPLE_FUJI = new Fruit("APPLE_FUJI", 0);
    public static final Fruit APPLE_PIPPIN = new Fruit("APPLE_PIPPIN", 1);

    private Fruit(String name, int ordinal) {
        super(name, ordinal);
    }
}
```

<br>

#### 3. 열거 타입은 밖에서 접근할 수 있는 생성자를 제공하지 않으므로 사실상 final이다.
```java
public enum Fruit {
    APPLE_FUJI, 
    APPLE_PIPPIN;

    private Fruit() {} // 생성자는 자동으로 private
}
```
모든 열거형의 생성자는 private이며, 외부에서 new로 생성 불가능하다. <br>
따라서, 새로운 인스턴스를 추가할 수 없고, 기존 인스턴스만 사용할 수 있다. <br>

```java
Fruit apple = new Fruit();  // ❌ 컴파일 에러! 열거 타입은 직접 인스턴스 생성 불가능
```
enum을 사용하면 싱글톤 패턴을 자연스럽게 적용 가능하다. <br><br>

#### 4. 열거 타입 선언으로 만들어진 인스턴스들은 딱 하나씩만 존재한다.
열거형 인스턴스는 단 한 번만 생성되며, 추가로 생성될 수 없다. <br>
즉, 동일한 인스턴스가 언제나 재사용된다. <br>
```java
public class Main {
    public static void main(String[] args) {
        Fruit a = Fruit.APPLE_FUJI;
        Fruit b = Fruit.APPLE_FUJI;

        System.out.println(a == b);  // true (같은 인스턴스)
    }
}
```
enum 인스턴스는 단 하나만 존재하므로 항상 == 비교가 가능하고, 불필요한 객체 생성을 방지한다. 

# 3. 열거 타입은 싱글턴을 일반화한 형태다.
enum은 싱글턴 패턴을 일반화한 형태다.
+ 열거 타입은 인스턴스가 통제된다.
+ 싱글턴은 원소가 하나뿐인 열거 타입이라고 할 수 있다.

### 1. 열거 타입은 컴파일타임 타입 안정성을 제공한다.
enum 덕분에 컴파일 시점에 오류를 잡을 수 있어 안전하다. <br>
![image](https://github.com/user-attachments/assets/3e838b0e-8d89-4392-bfd0-ece04b5502ec) <br><br>

### 2. 상수 값이 클라이언트로 컴파일되어 각인되지 않는다.
```java
public enum Fruit {
    APPLE, ORANGE;
}
```
정수 열거 패턴은 컴파일 시 상수가 정수값으로 박혀버려서, 상수 값이 바뀌면 클라이언트 코드까지 다시 컴파일해야 한다. <br>
그러나 열거 타입은 상수 값이 아닌 이름만 공개하므로, 열거 타입 내부 구현이 바뀌어도 클라이언트 코드를 다시 컴파일할 필요가 없다. <br>
```java
// 정수 열거 패턴
public static final int APPLE = 0;  // 이 값이 바뀌면 클라이언트도 컴파일 필요!

// 열거 타입
public enum Fruit {
    APPLE, ORANGE;  // 내부 값이 바뀌어도 클라이언트에는 영향 없음
}
```

<br><br>

### 3. 열거 타입은 메서드, 필드를 추가하고 인터페이스를 구현할 수 있다.
enum은 임의의 메서드와 필드 추가, 인터페이스 구현이 가능하다. <br>
```java
// 인터페이스 구현: Comparable 인터페이스 구현으로 비교 기능 제공
public enum Fruit implements Comparable<Fruit> {
    APPLE(1), ORANGE(2), BANANA(3);

    private final int rank; // 필드 추가

    Fruit(int rank) {
        this.rank = rank;
    }

    // 메서드 추가
    public int getRank() {
        return rank;
    }
}
```

# 4. 데이터와 메서드를 갖는 열거 타입
열거 타입 상수 각각을 특정 데이터와 연결지으려면 생성자에서 데이터를 받아 인스턴스 필드에 저장하면 된다. <br><br>
### 예시
Planet의 생성자에서 표면중력을 계산해 저장했다.질량과 반지름이 있으니 표면중력은 언제든 계산 가능하지만 최적화를 위해 생성자에 저장했다.  
```java
package effectivejava.chapter6.item34;

public enum Planet {
    MERCURY(3.302e+23, 2.439e6),
    VENUS  (4.869e+24, 6.052e6),
    EARTH  (5.975e+24, 6.378e6),
    MARS   (6.419e+23, 3.393e6),
    JUPITER(1.899e+27, 7.149e7),
    SATURN (5.685e+26, 6.027e7),
    URANUS (8.683e+25, 2.556e7),
    NEPTUNE(1.024e+26, 2.477e7);

    private final double mass;           // 질량: In kilograms
    private final double radius;         // 반지름: In meters
    private final double surfaceGravity; // 표면중력: In m / s^2

    // 중력상수:  in m^3 / kg s^2
    private static final double G = 6.67300E-11;

    // Constructor
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
        surfaceGravity = G * mass / (radius * radius);
    }

    public double mass()           { return mass; }
    public double radius()         { return radius; }
    public double surfaceGravity() { return surfaceGravity; }

    public double surfaceWeight(double mass) {
        return mass * surfaceGravity;  // F = ma
    }
}
```

#### 1. 열거 타입은 근본적으로 불변이라 모든 필드는 final이어야 한다.
#### 2. 필드를 public 으로 선언해도 되지만, private으로 두고 별도의 public 접근자 메서드를 두는게 낫다.

<br>

어떤 객체의 지구에서의 무게를 입력받아 여덟 행성에서의 무게를 출력하는 일을 다음처럼 짧은 코드로 작성할 수 있다. <br>
![image](https://github.com/user-attachments/assets/5db5cf5d-2533-43a2-b429-e4c212098fd2)

### 열거타입의 배열
위 실행문을 통해 알 수 있듯이 <br>
열거 타입은 자신 안에 정의된 상수들의 값을 배열에 담아 반환하는 정적 메서드인 values를 제공한다. <br>
각 열거 타입 값의 toString 메서드는 상수 이름을 문자열로 반환하므로 println과 printf로 출력하기에 좋다. <br><br><br>

### 열거타입을 올바르게 쓰기
열거 타입을 사용할 때의 접근 제어 방식을 적절히 선택하는 것이 중요하다.
#### 그 기능을 클라이언트에 노출해야 할 합당한 이유가 없다면, private으로 한다. (필요하면, package-private으로 선언하자)
CarType은 Car 클래스 외부에서 사용될 필요가 없기 때문에 **private**으로 선언했다. <br>
Car 클래스 내에서만 사용되며, CarType을 Car의 내부 기능으로 제한한다.
```java
public class Car {
    private enum CarType {
        SEDAN, SUV, COUPE;
    }

    private CarType type;

    public Car(CarType type) {
        this.type = type;
    }

    public CarType getType() {
        return type;
    }
}
```

<br>

#### 널리 쓰이는 열거 타입은 톱레벨 클래스로 만든다.
Day 열거 타입은 여러 클래스에서 사용될 수 있는 공통된 값이므로 톱레벨에 정의한다. <br>
```java
public enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}

public class Schedule {
    private Day day;

    public Schedule(Day day) {
        this.day = day;
    }

    public String getDayMessage() {
        switch (day) {
            case MONDAY: return "Start of the work week!";
            case FRIDAY: return "TGIF!";
            default: return "Just another day.";
        }
    }
}
```

<br>

#### 특정 톱레벨 클래스에서만 쓰인다면 해당 클래스의 멤버 클래스로 만든다.
Signal 열거 타입은 TrafficLight 클래스 내부에서만 사용되기 때문에 멤버 클래스로 선언했다.  <br>
이 열거 타입은 TrafficLight의 상태를 추적하는 데 사용된다. <br>
```java
public class TrafficLight {
    private enum Signal {
        RED, YELLOW, GREEN;
    }

    private Signal currentSignal;

    public TrafficLight(Signal initialSignal) {
        this.currentSignal = initialSignal;
    }

    public void changeSignal() {
        switch (currentSignal) {
            case RED: currentSignal = Signal.GREEN; break;
            case GREEN: currentSignal = Signal.YELLOW; break;
            case YELLOW: currentSignal = Signal.RED; break;
        }
    }

    public Signal getCurrentSignal() {
        return currentSignal;
    }
}
```

# 5. 상수별 메서드 구현(constant-specific method implementa-tion)
열거 타입에 apply라는 추상 메서드를 선언하고 강 상수별 클래스 몸체(constant-specific class body), 즉 각 상수에 자신에 맞게 재정의하는 방법이다.

```java
package effectivejava.chapter6.item34;
import java.util.*;
import java.util.stream.Stream;

import static java.util.stream.Collectors.toMap;

public enum Operation {
    PLUS("+") {
        public double apply(double x, double y) { return x + y; }
    },
    MINUS("-") {
        public double apply(double x, double y) { return x - y; }
    },
    TIMES("*") {
        public double apply(double x, double y) { return x * y; }
    },
    DIVIDE("/") {
        public double apply(double x, double y) { return x / y; }
    };

    private final String symbol;

    Operation(String symbol) { this.symbol = symbol; }

    @Override public String toString() { return symbol; }

    public abstract double apply(double x, double y);

    // Implementing a fromString method on an enum type (Page 164)
    private static final Map<String, Operation> stringToEnum =
            Stream.of(values()).collect(
                    toMap(Object::toString, e -> e));

    // Returns Operation for string, if any
    public static Optional<Operation> fromString(String symbol) {
        return Optional.ofNullable(stringToEnum.get(symbol));
    }

    public static void main(String[] args) {
        double x = Double.parseDouble(args[0]);
        double y = Double.parseDouble(args[1]);
        for (Operation op : Operation.values())
            System.out.printf("%f %s %f = %f%n",
                    x, op, y, op.apply(x, y));
    }
}
```
상수별 메서드 구현을 상수별 데이터와 결합할 수도 있다.  <br>
위의 코드처럼, Operation의 toString을 재정의해 해당 연산을 뜻하는 기호를 반환할 수 있다. <br> 
2와 4를 주어 프로그램을 실행하면 다음 결과를 볼 수 있다. <br>
![image](https://github.com/user-attachments/assets/72d4cd18-aaa0-489c-b688-b130ba097f77)

## 열거 타입용 fromString 메서드 구현하기
다음 코드는 열거 타입에서 사용할 수 있도록 구현한 fromString이다.
```java
   private static final Map<String, Operation> stringToEnum =
            Stream.of(values()).collect(
                    toMap(Object::toString, e -> e));

    // 지정한 문자열에 해당하는 Operation을 (존재한다면) 반환한다.
    public static Optional<Operation> fromString(String symbol) {
        return Optional.ofNullable(stringToEnum.get(symbol));
    }
```

일단 코드를 읽어보자. <br>
#### stringToEnum 맵
Operation 상수들이 stringToEnum 맵에 추가되는 시점은 열거 타입 상수 생성 후 정적 필드가 초기화될 때다. <br> 
즉, 열거 타입 상수들이 생성자에서 초기화된 후, 정적 필드들이 초기화된다. <br>
> 위 코드에서는 Stream.of(values())를 사용해 Operation 상수들을 스트림으로 처리하고, 이를 맵에 추가한다. <br>
> 자바 8 이전에는 values() 메서드가 반환하는 배열을 순회하여 맵에 {문자열, 열거 타입 상수} 쌍을 수동으로 추가했을 것이다.

> 만약 stringToEnum 맵을 열거 타입 생성자에서 초기화하려고 시도하면, <br>
> 정적 필드들이 초기화되지 않았기 때문에 컴파일 오류가 발생할 수 있다. (**런타임에 NullPointerException**이 발생)

<br>

#### fromString 메서드
주어진 문자열(symbol)을 사용하여 stringToEnum 맵에서 해당하는 값을 검색하고, 존재하면 Optional<Operation>을 반환한다. <br>
만약 존재하지 않으면 Optional.empty()를 반환하여 null을 안전하게 처리할 수 있도록 한다. <br>

> fromString 메서드는 주어진 문자열이 해당하는 열거 타입 상수가 없을 수 있음을 클라이언트에게 알리고, <br>
> 이 경우 클라이언트에서 적절히 처리할 수 있도록 한다. <br>
> 이를 위해 반환 타입은 Optional<Operation>으로 처리하여 null 대신 Optional.empty()를 반환한다.

<br>

### 열거 타입의 toString 메서드 재정의 시의 고려사항
위의 코드처럼, 열거 타입의 toString 메서드를 재정의하려면 다음을 고려해야 한다. <br>
해당 toString() 값을 다시 해당 열거 상수로 변환할 수 있는 fromString 메서드도 함께 제공하는 것이 좋다. <br>
**toString**이 반환하는 문자열은 각 상수마다 고유해야 하며, 이 값이 정확하게 일치하도록 해야 한다.

<br>

### fromString에서 Optional을 반환하는 점도 주의하자.
fromString 메서드는 **Optional<Operation>**을 반환하여, 주어진 문자열이 유효한 열거 타입 상수에 매핑되지 않음을 클라이언트에 알려준다. <br>
이를 통해 클라이언트는 이 상황을 적절히 처리할 수 있다. 예를 들어, "UNKNOWN"과 같은 문자열을 입력했을 때 이를 처리할 방법을 제공할 수 있다.

# 6. 전략 열거 타입 패턴
전략 열거 타입 패턴은 전략 패턴을 열거 타입을 사용하여 구현한 방식이다. <br>
여기서 열거 타입의 각 상수가 다양한 전략을 구현하는 객체 역할을 하며, 클라이언트는 이를 통해 특정 전략을 선택하고 사용할 수 있다. 
```java

enum PayrollDay {
    FRIDAY(PayType.WEEKDAY), MONDAY(PayType.WEEKDAY), SATURDAY(PayType.WEEKEND),
    SUNDAY(PayType.WEEKEND), THURSDAY(PayType.WEEKDAY),
    TUESDAY(PayType.WEEKDAY), WEDNESDAY(PayType.WEEKDAY);

    private final PayType payType;

    PayrollDay(PayType payType) {
        this.payType = payType;
    }

    int pay(int minutesWorked, int payRate) {
        return payType.pay(minutesWorked, payRate);
    }

    // 전략 열거 타입!
    enum PayType {
        WEEKDAY {
            int overtimePay(int minsWorked, int payRate) {
                return minsWorked <= MINS_PER_SHIFT ? 0 :
                        (minsWorked - MINS_PER_SHIFT) * payRate / 2;
            }
        },
        WEEKEND {
            int overtimePay(int minsWorked, int payRate) {
                return minsWorked * payRate / 2;
            }
        };

        abstract int overtimePay(int mins, int payRate);

        private static final int MINS_PER_SHIFT = 8 * 60;

        int pay(int minsWorked, int payRate) {
            int basePay = minsWorked * payRate;
            return basePay + overtimePay(minsWorked, payRate);
        }
    }

    public static void main(String[] args) {
        for (PayrollDay day : values())
            System.out.printf("%-10s%d%n", day, day.pay(8 * 60, 1));
    }
}
```
각 요일에 따른 급여 계산 전략을 클래스가 아닌 열거 타입으로 구현하여, <br>
전략이 변경될 때마다 열거 타입 상수를 수정하는 방식으로 전략을 교체할 수 있다.
PayType 열거 타입은 다양한 전략을 캡슐화하여 코드의 확장성을 높이고, 새로운 급여 계산 전략을 추가할 때 열거 타입 상수만 추가하면 된다.

# 7. 열거타입은 언제 써야 할까?
#### 1. 필요한 원소를 컴파일타임에 알 수 있는 상수 집합이라면 항상 열거 타입을 사용하자.
태양계 행성, 한 주의 요일, 체스 말처럼 본질적으로 열거 타입인 타입은 당연히 포함된다.

#### 2. 열거 타입에 정의된 상수 개수가 영원히 고정 불변일 필요는 없다.
열거 타입은 나중에 상수가 추가돼도 바이너리 수준에서 호환되도록 설계되었다.

# 핵심정리
열거 타입은 정수 상수보다 안전하고 강력하다. 주요 포인트는 다음과 같다. <br>
+ 각 상수를 특정 데이터와 연결하거나 상수마다 다른 동작을 할 때는, 명시적 생성자나 메서드를 사용해야 한다.
+ 하나의 메서드가 상수별로 다르게 동작할 경우, switch 문 대신 상수별 메서드 구현을 사용하는 것이 좋다.
+ 열거 타입 상수 일부가 같은 동작을 공유할 경우, 전략 열거 타입 패턴을 사용하여 코드의 유연성과 확장성을 높일 수 있다.
  

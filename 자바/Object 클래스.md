자바가 기본적으로 제공하는 라이브러리 중에 가장 기본이 되는 것이 바로 java.lang 패키지이다. <br>
여기서 lang은 Language의 줄임말이다. <br><br>
java.lang 패키지의 대표적인 클래스들을 다음과 같다. <br>

+ Object : 모든 자바 객체의 부모 클래스
+ String : 문자열
+ Integer , Long , Double : 래퍼 타입, 기본형 데이터 타입을 객체로 만든 것
+ Class : 클래스 메타 정보
+ System : 시스템과 관련된 기본 기능들을 제공

<br>

이번시간에는 Object 클래스에 대해 정리해보자. <br>

## Object 클래스
+ extends 키워드로 다른 클래스를 상속하지 않으면, 암시적으로 java.lang.Object 클래스를 상속하게 된다.
+ 따라서 자바의 모든 클래스는 Object 클래스의 자식이거나 자손 클래스다.
+ Object는 자바의 최상위 부모 클래스이다.

## Object클래스의 equals()메소드
equals() 메서드 코드는 다음과 같다. 
```java 
public boolean equals(Object obj) {...}
```

+ 매개 타입은 Object이다.
+ 모든 객체가 매개값으로 대입될 수 잇다.  <br>  왜냐하면, 모든 객체는 Object타입으로 자동 타입 변환될 수 있기 때문이다. <br>
+ `비교 연산자인 ==과 같은 결과를 리턴한다.` (두 객체가 동일한 객체라면 true)
+ 일반적으로 Object의 equals()메소드는 직접 사용되지 않고 하위 클래스에서 재정의하여 논리적으로 동등 비교할 때 이용된다.

<br>

### String객체의 equals() 메소드
+ `문자열이 동일한지 조사한다.` (객체의 번지 비교가 아니라)
+ String 클래스가 Object의 equals() 메소드를 재정의(오버라이딩)해서 번지 비교가 아닌 문자열 비교로 변경했기 때문이다.

 <br>
 
### equals() 메소드 재정의
+ 매개값(비교 객체)이 기준 객체와 `동일한 타입의 객체인지` 확인한다.
+ Object 타입의 매개 변수는 모든 객체가 매개값으로 제공될 수 있기 때문에 <br> `instanceOf 연산자`로 기준 객체와 동일한 타입인지 제일 먼저 확인해야 합니다.
+ 비교 객체가 동일한 타입이라면, 기준 객체 타입으로 강제 타입 변환해서 필드값이 동일한지 검사하면 된다. <br> 
+ 필드값이 모두 동일하다면 true를 리턴하고, 그렇지 않으면 false를 리턴한다.

<br>

### 예제: 객체 동등 비교 equals() 메소드
의도한 대로 "홍길동"을 읽으려면, 다음과 같이 재정의한 hashCode() 메소드를 추가해야 한다. <br>
+ hashCode()의 리턴값을 number필드값으로 했기 때문에, 저장할 때 읽을 때의 같은 해시코드가 리턴된다.
+ 저장할 때 new Key(1), 읽을 때의 new Key(1)은 사실 서로 다른 객체이지만, <br> HashMap은 hashCode()의 리턴값이 같고 equals()의 리턴값이 true가 되기 때문에 <br> 동등한 객체로 인식한다는 뜻이다.
+ 객체의 동등 비교를 위해서는, Object의 equals()와 hashCode()메소드를 재정의해야 한다.
```java
import java.util.HashMap;

class Key {
    public int number;

    public Key(int number) {
        this.number = number;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Key) {
            Key compareKey = (Key) obj;
            if(this.number == compareKey.number) {
                return true;
            }
        }
        return false;
    }

    @Override
    public int hashCode() { //리턴값을 number로 했다 
        return number; //--> 저장할 때의 new Key(1), 읽을 때의 new Key(1)으로, --> 같은 해시코드가 리턴된다.
    }
}

public class Main {
    public static void main(String[] args) {
        //Key 객체를 식별키로 사용해서 String값을 저장하는 HashMap 객체 생성하기
        HashMap<Key, String> hashMap = new HashMap<Key, String>();
        
        hashMap.put(new Key(1), "홍길동"); //식별키 new Key(1)로 "홍길동"을 저장한다.

        String value = hashMap.get(new Key(1)); //식별키 new Key(1)로 "홍길동"을 읽어온다.
        System.out.println(value); // print "홍길동"

    }
}
```

<br>

### 예제: Member 클래스에 추가해보기
Member를 관리하기 위해서 오버라이딩 로직을 구현하려고 한다. <br>
id 필드값이 같은 경우, 같은 해시코드를 리턴하도록 한다.
+ String의 hashCode()메소드의 리턴값을 활용한다.
+ String의 hashCode()는 같은 문자열일 경우, 동일한 해시코드를 리턴한다.

```java
import java.util.HashMap;

class Member {
    public String id;

    public Member(String id) {
        this.id = id;
    }

   // 동등성 비교
    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member) {
            Member member = (Member) obj;
            if (id.equals(member.id)) {
                return true;
            }
        }
        return false;
    }

    @Override
    public int hashCode() { //id가 동일한 문자열인 경우, 같은 해시코드를 리턴한다.
        return id.hashCode();
    }
}

public class Main {
    public static void main(String[] args) {
        HashMap<Member, String> hashMap = new HashMap<>();

        hashMap.put(new Member("name"), "홍길동");

        String value = hashMap.get(new Member("name"));
        System.out.println(value); // print "홍길동"

    }
}
```



## Object 클래스의 toString()메소드
+ 객체의 문자 정보를 리턴한다.
    + 객체의 문자 정보 :  객체를 문자열로 표현한 것
    + Object클래스의 toString() 메소드는 '클래스이름@16진수해시코드'로 구성된 문자 정보를 리턴한다.
+ java.util패키지의 Date클래스는 toString()메소드를 재정의하여 '현재 시스템의 날짜와 시간 정보'를 리턴한다. 
+ String클래스는 toString()메소드를 재정의해서 저장하고 있는 문자열을 리턴한다.

### 예제 : SmartPhone 클래스에서 toString()메소드를 재정의
```java
class SmartPhone {
    private String company;
    private String os;

    public SmartPhone(String company, String os) {
        this.company = company;
        this.os = os;
    }

    @Override
    public String toString() {
        return company + ", " + os;
    }
}

public class Main {
    public static void main(String[] args) {
        SmartPhone myPhone = new SmartPhone("구글", "안드로이드");

        String strObj = myPhone.toString(); //재정의된 toString() 호출
        System.out.println(strObj);

        System.out.println(myPhone); //객체인 경우, 재정의된 toString()을 호출하고 리턴값을 받아 출력한다.
    }
}
```

#### 결과
```
구글, 안드로이드
구글, 안드로이드
```

<br>

### Object와 OCP
#### OCP 원칙
+ Open: 새로운 클래스를 추가하고, toString() 을 오버라이딩해서 기능을 확장할 수 있다.
+ Closed: 새로운 클래스를 추가해도 Object 와 toString()을 사용하는 클라이언트 코드는 변경하지 않아도 된다.

다형적 참조, 메서드 오버라이딩, 그리고 클라이언트 코드가 구현체 코드에 의존하는 것이 아니라 <br>
추상적인 Object에 의존하면서 OCP 원칙을 지킬 수 있었다. <br>
덕분에 새로운 클래스를 추가하고 toString() 메서드를 새롭게 오버라이딩해서 기능을 확장할 수 있다.

#### 예시
객체 지향 설계를 위해 추상화된 Printable 인터페이스를 정의해보자.  <br>
그런 다음, 이 인터페이스를 구현하는 구체적인 클래스인 Car와 Dog을 만든다. <br>
ObjectPrinter 클래스를 작성하여 다양한 객체를 출력할 수 있다. <br><br>


```java
package objects;

// 객체를 출력할 때 사용할 인터페이스를 정의한다.
public interface Printable {

    //출력을 위한 문자열 반환 메서드를 선언한다.
    String toString();
}
```

```java
package objects;

// Printable 인터페이스를 구현하는 자동차 클래스를 정의한다.
public class Car implements Printable {
    private String make;
    private String model;

    // 자동차의 제조사와 모델을 초기화하는 생성자를 정의한다.
    public Car(String make, String model) {
        this.make = make;
        this.model = model;
    }

    // Printable 인터페이스의 toString() 메서드를 오버라이드하여 자동차 정보를 반환한다.
    @Override
    public String toString() {
        return "Car: " + make + " " + model;
    }
}
```

```java
package objects;

// Printable 인터페이스를 구현하는 개 클래스를 정의한다.
public class Dog implements Printable {
    private String name;
    private String breed;

    // 개의 이름과 견종을 초기화하는 생성자를 정의한다.
    public Dog(String name, String breed) {
        this.name = name;
        this.breed = breed;
    }

    // Printable 인터페이스의 toString() 메서드를 오버라이드하여 개 정보를 반환한다.
    @Override
    public String toString() {
        return "Dog: " + name + " (" + breed + ")";
    }
}
```

```java
package objects;

// Printable 객체를 출력하는 유틸리티 클래스를 정의한다.
public class ObjectPrinter {
    // Printable 객체를 받아들여 문자열로 출력한다.
    public static void print(Printable printable) {
        System.out.println(printable);
    }
}
```

```java
package objects;

public class Main {
    public static void main(String[] args) {
        // Printable 인터페이스를 구현한 Car와 Dog 객체를 생성한다.
        Car myCar = new Car("Toyota", "Corolla");
        Dog myDog = new Dog("Max", "Labrador");

        // ObjectPrinter를 통해 Printable 객체를 출력한다.
        ObjectPrinter.print(myCar);
        ObjectPrinter.print(myDog);
    }
}
```

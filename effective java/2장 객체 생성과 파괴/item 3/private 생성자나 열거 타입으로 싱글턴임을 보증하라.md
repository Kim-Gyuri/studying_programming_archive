# private 생성자나 열거 타입으로 싱글턴임을 보증하라

### 포인트
singleton이란 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다. 싱글톤의 전형적인 예로는 함수와 같은 무상태 객체나 설계상 유일해야 하는 시스템 컴포넌트를 들 수 있다. <br>
그런데 `클래스를 싱글톤으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다` <br> 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 만든 싱글톤이 아니라면, <br>
싱글톤 인스턴스를 가짜(mock) 구현으로 대체할 수 없기 때문이다. <br><br>
싱글톤을 만드는 방식은 보통 둘 중 하나다. 두 방식 모두 생성자는 private으로 감춰두고, 유일한 인스턴스에 접근할 수 있는 수단으로 public static 멤버를 하나 마련해둔다.

## 싱글톤 (singleton)
인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.


### public으로 만들었을 때 문제점
```java
// 싱글톤 패턴코드
package effectiveJava.item3;

public class Singleton {

    public static Singleton instance;

    //기본 생성자를 작성하지 않는 경우
    // -> public으로 만들어진다.

    public static Singleton getInstance() {
        if (null == instance) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

```java
package effectiveJava.item3;

public class Main {
    public static void main(String[] args) {
	// getInstance()로만 접근해야 하는데, Singleton.instance로 접근할 수 있어서 문제가 된다.
        Singleton singleton = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();
        Singleton singleton3 = Singleton.instance;

        System.out.println(singleton); //effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton2); //effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton3); //effectiveJava.item3.Singleton@3b07d329

    }
}
```

---
### private 으로 막았지만 아직 문제가 남았다.
직접 생성이 가능한 문제가 있다.
```java
package effectiveJava.item3;

public class Singleton {
    private static Singleton instance; //private으로 감췄을 때

   //-> 여기서 생성자를 만들어 두지 않았기 때문에
   // 자바가 default 생성자를 만들어 둔다.
   // public class Singleton의 public 접근제한자를 따른 기본 생성자가 만들어졌다.
  // -> 그래서 Singleton singleton3 = new Singleton(); 이렇게 접근되는 문제가 있다.

    public static Singleton getInstance() {
        if (null == instance) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

```java
package effectiveJava.item3;

public class Main {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();
        Singleton singleton2 = Singleton.getInstance();
        //Singleton singleton3 = Singleton.instance; -> private으로 감추면 이 코드는 접근 제한이 걸린다.
        Singleton singleton3 = new Singleton(); // -> 여기서 기본 생성자에 의해 인스턴스 접근할 수 있다


        System.out.println(singleton);    // effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton2); // effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton3); // effectiveJava.item3.Singleton@41629346 
    }
}
```

---
### private 접근제한 기본 생성자를 만들었을 때
이제 Singleton singleton3 = new Singleton(); 을 막을 수 있게 되었다.
```java
package effectiveJava.item3;

public class Singleton {
    private static Singleton instance;

    private Singleton() {} // 기본 생성자 추가

    public static Singleton getInstance() {
        if (null == instance) {
            instance = new Singleton();
        }

        return instance;
    }
}
```

#### 이때 문제는 다음과 같다.
```java
package effectiveJava.item3;

public class Singleton {
    private static Singleton instance;

    private Singleton() {} 

    public static Singleton getInstance() {
        if (null == instance) { // -> 여기서 문제가 발생할 여지가 있다.
            instance = new Singleton();
        }

        return instance;
    }
}
```

---
### 싱글톤 인스턴스에 접근하는 적군 경합 Data Race가 생길 수도 있다.
즉, 다른 인스턴스가 생길 수도 있는 문제가 있다.
```java
package effectiveJava.item3;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Runnable runnable = () -> {
            Singleton singleton = Singleton.getInstance();
            System.out.println(singleton);
        };

        List<Thread> threadList = new ArrayList<>();
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));
        threadList.add(new Thread(runnable));

        for (Thread thread : threadList) {
            thread.start();
        }
    }
}
```

#### 출력
(n번 실행하다보면) 인스턴스가 위의 출력처럼 달라짐을 확인할 수 있다.
```
effectiveJava.item3.Singleton@512e4218
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@7d0d4b30
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
effectiveJava.item3.Singleton@167c8476
```

---
###  if (null == instance) 코드 부분에서 문제점은
필드의 instance가 null일 경우 싱글톤을 생성함으로써  lazy initialization 처리해준다. <br>
그러나, `멀티 프로세싱이 되는 환경에서는 문제가 된다. thread safe하지 않다.` <br> 아래와 같이 회피하는 방법도 있다.

```java
// synchronized를 입력해서 고유락(Intrinsic Lock)을 걸어서
하나씩만 접근하게 유도할 수 있다. (같은 인스턴스를 반환한다.)
public static synchronized Singleton getInstance() {
        if (null == instance) {
            instance = new Singleton();
        }

        return instance;
    }
```

---
### Double-Checked Locking 
synchronized는 매번 접근할 때 비용이 발생하기 때문에, Double-Checked Locking으로  락의 범위를 처음에 경합되는 상황에만 좁힌다고 한다.  
```java
package effectiveJava.item3;

public class Singleton {
 //-> volatile 키워드를 붙여줘야한다.
 // 메모리 가시성 문제가 있기  때문이다,
// 메모리 가시성에 접근에 대한 부분 (캐시, cpu 속도 이슈)를 반영하여 
// 메모리 직접 접근할 수 있는 volatile을 적어야 한다.
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (null == instance) {
            // 여기서 한번 더 체크를 하는 것을 말한다.
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

---
# 싱글톤을 만드는 방식
## 첫번째 방법: private 생성자 + public static final 필드인 방식을 살펴보자.
```java
package effectiveJava.item3;

public class Singleton {
    public static final Singleton INSTANCE = new Singleton();
    private Singleton() {}

}
```

```java
package effectiveJava.item3;

import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Singleton singleton = Singleton.INSTANCE;
        Singleton singleton2 = Singleton.INSTANCE;
        Singleton singleton3 = Singleton.INSTANCE;
        Singleton singleton4 = Singleton.INSTANCE;

        System.out.println(singleton);    //effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton2); //effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton3); //effectiveJava.item3.Singleton@3b07d329
        System.out.println(singleton4); //effectiveJava.item3.Singleton@3b07d329
    }
}
```

### 장점
구현이 간결하고 싱글턴임을 API에 들어낼 수 있다.
>  public static 필드가 final이니 절대로 다른 객체를 참조할 수 없다.

### 단점
+ `1` 싱글톤을 사용하는 클라이언트가 테스트하기 어려워진다.
+ `2` 리플랙션으로 private 생성자를 호출할 수 있다.
+ `3` 역직렬화 할 때 새로운 인스턴스가 생길 수 있다.

## 2번째 방법: 정적 팩터리 메서드를 public static 멤버로 제공한다.
(클라이언트 입장에서는) getInstance()를 호출해서 싱글톤 하나를 반환받는다.
> getInstance()는 항상 같은 객체의 참조를 반환하므로, 제2의 인스턴스가 생성되지 않는다.

```java
package effectiveJava.item3;

public class Singleton {
    // public static final이니 절대로 다른 객체를 참조할 수 없다.
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() {}
    // 정적 팩터리 메서드를 public static 멤버로 제공한다.
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

```java
package effectiveJava.item3;

public class Main {
    public static void main(String[] args) {
        Singleton singleton = Singleton.getInstance();

        Singleton singleton2 = Singleton.getInstance();
        Singleton singleton3 = Singleton.getInstance();
        Singleton singleton4 = Singleton.getInstance();

        System.out.println(singleton);
        System.out.println(singleton2);
        System.out.println(singleton3);
        System.out.println(singleton4);
    }
}
```

### 방법1과 단점은 동일하다.

### 장접1 :API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있다.
유일한 인스턴스를 반환하던 팩터리 메서드가 호출하는 스레드별로 다른 인스턴스를 넘겨주게 할 수 있다.
```java
public class MethodSingleton {
    private MethodSingleton() {}

    public static MethodSingleton getInstance() {
        return new MethodSingleton();
    }
}
```

### 장점2 : 정적 팩토리를 제네릭 싱글턴 팩터리로 만들 수 있다.
Generic 을 활용하여 같은 인스턴스를 반환하지만 타입을 변환하여 사용할 수 있다.
```java
package effectiveJava.item3;

public class Singleton<T> {
    private static final Singleton<?> INSTANCE = new Singleton<>();
    private Singleton() {}
    public static <T> Singleton<T> getInstance() {
        return new Singleton<>();
    }

    public boolean send(T message) {
        System.out.println(message);
        return true;
    }
}

        Singleton singleton = Singleton.getInstance();

        Singleton singleton2 = Singleton.getInstance();
        Singleton singleton3 = Singleton.getInstance();
        Singleton singleton4 = Singleton.getInstance();

        System.out.println(singleton);
        System.out.println(singleton2);
        System.out.println(singleton3);
        System.out.println(singleton4);
```

### 장점 3. 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다.
Singleton::getInstance를 Supplier<Sigleton>으로 사용하는 식이다.

```java
package effectiveJava.item3;

import java.util.function.Supplier;

// Singleton을 쓸 수 있는 SingletonTest를 만들어 본다.
public class SingletonTest {
    public void start(Supplier<Singleton> supplier) {
        Singleton<Singleton> singleton = supplier.get();
        singleton.send("hello");
    }
}
```

```java
package effectiveJava.item3;

public class Main {
    public static void main(String[] args) {
        SingletonTest singletonTest = new SingletonTest();
     // 이렇게 호출해줄 수 있다.
        singletonTest.start(Singleton::getInstance);
    }
}
```

---
## 두 가지(첫, 두 번째) 방식의 문제점과 해결 방안
### 문제점 1 : 싱글턴을 사용하는 클라이언트가 테스트하기 어려워진다.
싱글턴을 사용하는 인스턴스가 비용(Money)이 발생하는 루틴이라고 가정하면, <br> 하나의 인스턴스만 생성되는 싱글턴의 특성상 대체가 곤란하다는 내용이다. <br> `인터페이스를 구현해서 만든 Mock으로 대체할 수 있다.` <br><br>

인터페이스를 만들었고, 
```java
package effectiveJava.item3;

public interface ISingle {
    boolean send(String message);
}
```

그것을 implement하여 싱글톤을 사용하면 비용이 발생한다.
```java
package effectiveJava.item3;

public class Singleton<T> implements ISingle {
    private static final Singleton INSTANCE = new Singleton<>();
    private Singleton() {}
    public static Singleton getInstance() {
        return new Singleton<>();
    }

   // -> 비용 발생
    public boolean send(String message) {
        System.out.println("cost: " + message);
        return true;
    }
}
```

#### 인터페이스를 구현해서 만든 Mock으로 대체할 수 있다.
그래서 Mock을 만들어 보면,
```java
// 가짜 객체 구현
package effectiveJava.item3;

public class MockSingleton implements ISingle {
    private static final MockSingleton INSTANCE = new MockSingleton();
    private MockSingleton() {}
    public static MockSingleton getInstance() {
        return new MockSingleton();
    }

   // 가짜 구현체를 이용하여 비용 발생하지 않음.
    @Override
    public boolean send(String message) {
        System.out.println("mock: " + message);
        return false;
    }
}
```

Mock싱글톤으로 대체해서 사용하면
```java
package effectiveJava.item3;

public class Main {
    public static void main(String[] args) {
        SingletonTest singletonTest = new SingletonTest();
        singletonTest.start(Singleton::getInstance);

        // 실 비즈니스 동작 시 사용
        ISingle singleton = Singleton.getInstance();
        singleton.send("example text");

       // 테스트 시 구현 대체 
       // ISingle을 참조하도록 한다. 그리고 MockSingleton으로 대체해서 작성한다.
        ISingle singleton2 = MockSingleton.getInstance();
        singleton2.send("example text2");
    }
}
```


## 또 다른 방법으로, SingletonTest에서 Isingle을 받도록 할 수 있다.
```java
package effectiveJava.item3;

import java.util.function.Supplier;

public class SingletonTest {
    public void start(Supplier<ISingle> supplier) {
        ISingle singleton = supplier.get();
        singleton.send("hello");
    }
}
```

인터페이스로 구현해서, 테스트 Mock 싱글톤으로 대체할 수 있다.
```java
package effectiveJava.item3;

public class Main {
    public static void main(String[] args) {
        SingletonTest singletonTest = new SingletonTest();
        //singletonTest.start(Singleton::getInstance);
        singletonTest.start(MockSingleton::getInstance);
    }
}
```

## 문제점 2 : 리플렉션에서 안전하지 못하다.
리플렉션에서 취악한지 실행해보자.

```java
package study.effectivejava.item3;

public class MethodSingleton {
    private static final MethodSingleton INSTANCE = new MethodSingleton();

    public static MethodSingleton getInstance() {
        return new MethodSingleton();
    }
}
```

```java
package study.effectivejava.item3;

import lombok.SneakyThrows;

import java.lang.reflect.Constructor;

public class ReflectionAttack {

    @SneakyThrows
    public static <T> T getNewInstance(Class<?> clz) {
     // private 생성자를 자바의 리플랙션으로 접근하여, 생성자의 접근 제어자를 변경할 수 있다.
    // getDeclearedConstor()로 전체 선언을 가지고 와서, 접근 제어를 true로 바꿔준다.
        Constructor<?> declaredConstructor = clz.getDeclaredConstructor();
        declaredConstructor.setAccessible(true);
        T newInstance = (T) declaredConstructor.newInstance(); // 새로운 인스턴스 취득 시도

        return newInstance; // ->  실제로 리플랙션에서 취약한지 테스트해보자.
    }

    public static void doTest() {
        MethodSingleton instance = MethodSingleton.getInstance();
        MethodSingleton newInstance = getNewInstance(MethodSingleton.class); // 새로운 인스턴스 취득 시도

        System.out.println("reflection unsafety: " + instance + " : " + newInstance);
        // -> 출력
       //  reflection unsafety: study.effectivejava.item3.MethodSingleton@17a7cec2
       //  instance : study.effectivejava.item3.MethodSingleton@65b3120a     
    }
}
```

결과를 보면, reflection unsafety, instance 주소가 갈려진다. (즉, "리플랙션에서 안전하지 못하다")

### 리플랙션 보완하기 위해서, throw를 던져 접근을 막자.
리플렉션을 이용하여, 생성자에 접근 제어자를 수정하여 새로운 인스턴스를 생성할 수 있다. <br>
`공격을 방어하려면, 생성자에서 두 번째 객체가 생성될 때 예외를 던진다.`
```java
package study.effectivejava.item3;

public class MethodSingleton implements ISingle {
    private static final MethodSingleton INSTANCE = new MethodSingleton();
    private static boolean isCreated;  // 변수로 생성시도를 표시

    private MethodSingleton() {
        if (isCreated) {
	// 생성이 되었다면, 생성 재시도 시 예외를 던진다.
            throw new UnsupportedOperationException("already exists");
        }
        isCreated = true;
    }

    public static MethodSingleton getInstance() {
        return new MethodSingleton();
    }

    @Override
    public boolean send(String message) {
        System.out.println("mock: " + message);
        return false;
    }
}
```

다시 ReflectionAttack.doTest();를 시도하면 예외로 막힘을 확인 가능하다. 
```
Exception in thread "main" java.lang.UnsupportedOperationException: already exists
	at study.effectivejava.item3.MethodSingleton.<init>(MethodSingleton.java:9)
	at study.effectivejava.item3.MethodSingleton.getInstance(MethodSingleton.java:15)
	at study.effectivejava.item3.ReflectionAttack.doTest(ReflectionAttack.java:19)
	at study.effectivejava.item3.Main.main(Main.java:5)
```

<br>

### 문제점 3 : 역직렬화 시 새로운 인스턴스가 생길 수 있다.
역직렬화 시 필드 변수의 값을 복사한 새로운 인스턴스를 생성할 수 있는 문제가 있다. <br>
이를 방지하기 위해 필드 변수에는 `transient`로 선언하여 다른 인스턴스로 복제됨을 방지한다. <br>
또한 동일한 인스턴스를 생성 및 반환하도록 `readResolve()`를 구현한다.

```java
package study.effectivejava.item3;

import java.io.*;

public class Serialization {
	// 직렬화를 이용해서 파일을 작성해서,
    public static void serialize(Object obj, String fileName) {
        try (ObjectOutput out = new ObjectOutputStream(new FileOutputStream(fileName))) {
            out.writeObject(obj);
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    // 그 파일을 역직렬화한다.
    public static Object deserialize(String fileName) {
        try (ObjectInput in = new ObjectInputStream(new FileInputStream(fileName))) {
            return in.readObject();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
    }

    public static void doTest() {
        String fileName = "serial.obj";
	
	// 직렬화
        SerializationSingleton serialIns = SerializationSingleton.getInstance();
        serialize(serialIns, fileName);
        System.out.println(serialIns);
        serialIns.display();

	// 역직렬화
        SerializationSingleton deserializeIns = (SerializationSingleton) deserialize(fileName);
        System.out.println(deserializeIns);
        deserializeIns.display();

    }
}
```

```java
package study.effectivejava.item3;

import java.io.Serializable;

public class SerializationSingleton implements Serializable {
// static final
    private static final SerializationSingleton INSTANCE = new SerializationSingleton();
    private String name = "thumper";

  // private 기본 생성자
    private SerializationSingleton() {}
 
// getInstance()로 반환 받기
    public static SerializationSingleton getInstance() {
        return INSTANCE;
    }

    public void display() {
        System.out.println(this.name);
    }
}
```

```java
package study.effectivejava.item3;

public class Main {
    public static void main(String[] args) {
	// 실행
        Serialization.doTest();
    }
}
```

#### 결과
싱글톤이 깨져서, 역직렬화 했을 때 새로운 객체가 생성됨을 발견했다.
```
study.effectivejava.item3.SerializationSingleton@1376c05c // 값이 다르다
thumper 
study.effectivejava.item3.SerializationSingleton@15615099 // 값이 다르다.
thumper
```

<br>

### 해결 코드 : 동일한 인스턴스를 생성 및 반환하도록 readResolve()를 구현한다.
```java
package study.effectivejava.item3;

import java.io.Serializable;

public class SerializationSingleton implements Serializable {
    private static final SerializationSingleton INSTANCE = new SerializationSingleton();
    //private String name = "thumper";
    private transient String name = "thumper";

    private SerializationSingleton() {}

    public static SerializationSingleton getInstance() {
        return INSTANCE;
    }

    public void display() {
        System.out.println(this.name);
    }

 // 해결 코드!
// 아래와 같이 정해진 코드로 동일한 인스턴스를 반환하도록 한다.
 // ovveride 코드도 아닌데, 시스템에서 지정된 로직으로 동작된다.
    private Object readResolve() {
        return INSTANCE;
    }
}
```

---
## 세번째 : 열거 타입
대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글톤을 만들기 최적화된 방법
```java
package study.effectivejava.item3;

public enum EnumSingleton implements ISingle {
    INSTANCE;


    @Override
    public boolean send(String message) {
        System.out.println("EnumSingleton");
        return false;
    }
}
```

```java
package study.effectivejava.item3;

public class Main {
    public static void main(String[] args) {
        EnumSingleton instance = EnumSingleton.INSTANCE;
        EnumSingleton instance1 = EnumSingleton.INSTANCE;
        EnumSingleton instance2 = EnumSingleton.INSTANCE;

        System.out.println(System.identityHashCode(instance));   // 122883338
        System.out.println(System.identityHashCode(instance1)); // 122883338
        System.out.println(System.identityHashCode(instance2)); // 122883338
    }
}
```

### 장점
더 간결하고, 추가 노력 없이 직렬화할 수 있고, <br> 심지어 아주 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 인스턴스가 생기는 일을 완벽히 막아준다.
> `대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글톤을 만드는 가장 좋은 방법이다.`

### 단점
장점 1,2를 가지지 못한다. 
> 만들려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면, 이 방법은 사용할 수 없다. <br> (열거 타입이 다른 인터페이스를 구현하도록 선언할 수는 있다.)

<br>

> 장접1 :API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있다. <br> (클라이언트 입장에서는) getInstance()를 호출해서 싱글톤 하나를 반환받는다. 

> 장점2 : 정적 팩토리를 제네릭 싱글턴 팩터리로 만들 수 있다.

<br><br><br>

> `번외` <br>
William Pugh가 고안한 이디엄 방식: 싱글톤을 안전하게 구현할 수 있다. <br>
Initialization-on-demand holder idiom (Bill Pugh Solution)

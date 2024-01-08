# finalizer와 cleaner 사용을 피하라.

### 결론
+ "finalizer, cleaner를 사용해서 객체 사용이 끝나는 시점에 소멸시키자" 라고 사용하지 말자.
+ GC의 대상이 되지만 바로 수거해가는게 아니다.

#### finalizer와 cleaner를 유용하게 사용할 일은 극히 드물다.
+ 안전망 역할로 자원을 반납하려 할 때 사용을 권한다.
+ 네이티브 자원을 정리할 때 사용을 권한다.

## finalizer와 cleaner 사용을 피해야 하는 이유
### 1. finalizer와 cleaner는 즉시 수행된다는 보장이 없다.
#### 객체에 접근할 수 없게 된 후 finalizer나  cleaner가 실행되기까지 얼마나 걸릴지 알 수 없다. 
`즉, finalizer와 cleaner로는 제때 실행되어야 하는 작업은 절대 할 수 없다.` <br>
> `예: 파일닫기` <br>
> 파일닫기를 finalizer나 cleaner에 맡기면 중대한 오류를 일으킬 수 있다. <br>
> 시스템을 동시에 열 수 있는 파일 개수에 한계가 있기 때문이다. <br>
> 시스템이 finalizer나 cleaner 실행을 게을리해서 파일을 계속 열어 둔다면, 새로운 파일을 열지 못해 프로그램이 실패할 수 있다.

#### 수행 시점이 전적으로 GC 알고리즘에 달려있으며, 구현 방식에 따라 천자만별이다.
finalizer 쓰레드는 다른 애플리케이션보다 우선순위가 낮다. (즉각 수행된다는 보장이 없다.) <br>
> Finalizer안에 작업이 있고 그 작업을 쓰레드가 처리하지 못 하고 대기하다가,  <br>
> 해당 인스턴스가 GC되지 않고 쌓이다가 결국 `OutOfMemoryError`를 발생할 수 있다.

---
### 2. 수행 여부도 보장하지 않는다.
#### 접근할 수 없는 객체에 딸린 종료작업을 수행하지 못한채 프로그램이 중단될 수도 있다.
#### `상태를 영구적으로 수정하는 작업에서는 절대 finalizer나 cleaner에 의존해서는 안 된다.`
예를 들어 데이터베이스 같은 공유 자원의 영구 락(lock) 해제를 finalizer나 cleaner에 맡겨 놓으면 분산 시스템 전체가 서서히 멈출 것이다. <br>
```java
@Override
protected void finalize() throws Throwable {
   // "lock 걸린거 이 객체가 소멸될 때 같이 lock을 해제하면 되겠다."라고 생각하면 안된다.
}
```

<br>

`System.gc나 System.runFunalization 메서드에 현혹되지 말자.` (실행 가능성은 높여주나 보장해주진않는다.) <br>
> 사실 보장해주겠다는 메서드가 2개 있었다. <br>
> Runtime.runFinalizersOnExit(boolean)와 그 쌍둥이인 System.Runtime.runFinalizersOnExit(boolean)다. <br>
> 하지만, 심각한 결함때문에 몇년간 deprecated 상태다. → java11에선 삭제되었다.

---
### 3. finalizer 동작 중 발생한 예외는 무시되며, 처리할 작업이 남았어도 그 순간 종료된다.
finalizer 동작 중 발생한 예외는 무시되며, 처리할 작업이 남아있더라도 그 순간 종료된다. <br>
잡지 못한 예외 때문에 해당 객체는 자칫 마루리가 덜 된 상태로 남을 수 있다. <br>
보통의 경우 잡지 못한 예외가 스레드를 중단시키고 스택 추적 내역을 출력한다. <br>
하지만 같은 일이 finalizer에서 발생한다면 경고조차 출력하지 않는다. <br><br>
그나마 cleaner를 사용하는 라이브러리는 자신의 쓰레드를 통제하기 때문에 이러한 문제가 발생하지 않는다

---
### 4. finalizer와 cleaner는 심각한 성능 문제도 동반한다.
+ AutoCloseable 객체를 생성하고 try-with-resources로 자원을 닫아서 가비지컬렉터가 수거하기까지 12ns 걸린다.
+ finalizer를 사용한 객체를 생성하고 파괴하니 550ns가 걸렸다. (50배, GC 효율을 떨어뜨린다.)
> finalizer가 가비지 컬렉터의 효율을 떨어지게 한다.   <br>
> 하지만 잠시 후 알아볼 안전망 형태로만 사용하면 66ns가 걸린다.  <br>
> 안전망의 대가로 50배에서 5배로 성능차이를 낼 수 있다.

---
### 5. finalizer를 사용한 클래스는 finalizer 공격에 노출되어 심각한 보안 문제를 일으킬 수 있다.
+ 생성자나 직렬화 과정에서 예외가 발생하면, 이 생성되다 만 객체에서 악의적인 하위 클래스의 finalizer가 수행될 수 있게 된다.
+ (finalizer를 사용하면 GC 효율을 떨어뜨려), 이렇게 일그러진 객체가 만들어지고 나면 <br> 
이 객체의 메서드를 호출해 애초에는 허용되지 않았을 작업도 수행 가능하다.
+ `객체 생성을 막으려면 생성자에서 예외를 던지는 것만으로 충분하지만, finalizer가 있다면 그렇지도 않다.`
+ final이 아닌 클래스를 finalizer 공격으로부터 방어하려면 아무 일도 하지 않는 finalize 메서드를 만들고 final로 선언하자.

#### 예제
이 코드 예제는 finalize 메소드를 사용하여 악의적인 하위 클래스를 만들어서 발생시킨 예외 상황을 보여주는 것으로 보인다. <br>
생성이나 직렬화 과정에서 에외가 발생하면 생성되다 만 객체에서 악의적인 하위 클래스의 finalizer가 수행될 수 있게 된다.

```java
package study.effectivejava.item8;

import java.text.MessageFormat;

/**
 * 은행을 나타내는  KakaoBank 클래스 정의
 */
public class KakaoBank {

    private int money; // 은행 잔고

    // 생성자: money가 1000원 미만일 경우 RuntimeException 발생한다.
    public KakaoBank(final int money) {
        if (money < 1000) {
            throw new RuntimeException("1000원 이하로 생성이 불가능해요.");
        }
        this.money = money;
    }

    // 입금 로직 : 돈을 입금한다.
    void transfer(final int money) {
        this.money -= money;
        System.out.println(MessageFormat.format("{0}원이 입금되었습니다.", money));
    }
}
```

```java
package study.effectivejava.item8;

/**
 * KakaoBank를 상속받은 악의적인 하위 클래스 BankAttack 정의
 */
public class BankAttack extends KakaoBank {

    // 생성자 : 상위 클래스의 생성자 호출
    public BankAttack(final int money) {
        super(money);
    }

    // finalize 메소드 오버라이딩 : 객체가 소멸될 때 호출되는 메서드
    @Override
    protected void finalize() throws Throwable {
        // finalize 메소드에서 10억원을 입금하는 메소드 호출한다
        this.transfer(1000000000);
    }
}
```

```java
package study.effectivejava.item8;

import static java.lang.Thread.sleep;

public class Main {
    public static void main(String[] args) throws InterruptedException {
        KakaoBank bank = null;  // 객체 생성
        try {
            // // BankAttack 객체를 생성하여 KakaoBank 변수에 할당
            bank = new BankAttack(500);
            // 입금 메서도 호출
            bank.transfer(1000);
        } catch (Exception e) {
            // 예외가 발생하면 예외 메시지 출력
            System.out.println("예외 발생!");
        }
        System.gc(); // GC 수행
        sleep(3000); // 3초 대기
    }
}
```

#### 결과출력
```
예외 발생!
1,000,000,000원이 입금되었습니다.
```

+ 이 코드는 BankAttack 클래스에서 finalize 메소드를 오버라이딩하여 KakaoBank의 transfer 메소드를 호출하고 있다. 
+ 그리고 Main 클래스에서 BankAttack 객체를 생성하고 입금을 시도한 후, 예외가 발생하면 "예외 터짐"을 출력하고, 가비지 컬렉션을 수행한 후 3초 동안 대기한다. 
+ finalize 메소드는 객체가 소멸될 때 호출되는데, 이 때 10억원을 입금하려고 시도하고 있다.
+ 하지만 finalize 메소드를 명시적으로 호출하는 것은 권장되지 않습니다. 이 코드는 finalize 메소드의 사용이 지양되어야 한다는 점을 강조하고 있다.

## finalizer, cleaner의 대안
`AutoCloseable`을 구현하고 클라이언트에서 인스턴스를 다 쓰고나면 close 메서드를 호출하면 된다. <br>
try-finally나 try-with-resourse를 사용하여 자원을 종료실킬 수 있다.

```java
public class Room implements AutoCloseable {
        @Override
        public void close() {
                System.out.println("close");
        }
}
```

### 1. 자원의 소유자가 close() 메서드를 호출하지 않는 것에 대비한 안전망 역할
cleaner, finalizer가 즉시 호출될것이란 보장은 없지만, 클라이언트가 하지 않은 자원 회수를 늦게라도 해주는 것이 아예 안하는 것보다 낫다. <br>
하지만 이런 안전망 역할로 finalizer를 작성할 때 그만한 값어치가 있는지 신중히 고려해야 한다.  <br>
자바에서는 안전망 역할의 finalizer를 제공한다. <br>
> 자바라이브러리 : FileInputStream, FileOutputStream, ThreadPoolExecutor

### 2.  네이티브 피어(native peer)와 연결된 객체
cleaner와 finalizer를 적절히 활용하는 두 번째 예는 네이티브 피어와 연결된 객체이다.  <br>
네이티브 피어란 일반 자바 객체가 네이티브 메서드를 통해 기능을 위임한 네이티브 객체를 말한다.  <br>
그 결과 자바 피어를 회수할 때 네이티브 객체까지 회수하지 못 한다.  <br>
성능 저하를 감당할 수 없거나 자원을 즉시 회수해야 한다면 close 메서드를 사용해야 한다. <br>

### 예제 
Room 클래스로 이 기능을 설명해보자. 방(room) 자원을 수거하기 전에 반드시 청소(clean)해야 한다고 가정해보자. <br>
Room 클래스는 AutoCloseable을 구현한다. 사실 자동 청소 안전망이 cleaner를 사용할지 말지는 순전히 내부 구현 방식에 관한 문제다. <br>
즉 finalizr와 달리 cleaner 클래스의 public API에 나타나지 않는다는 이야기다.

```java
package study.effectivejava.item8;

import java.lang.ref.Cleaner;

/**
 * cleaner를 안전망으로 활용하는 AutoCloseable 클래스
 */
public class Room implements AutoCloseable {
    private static final Cleaner cleaner = Cleaner.create();

    /**
     *  State : 청소(cleaning)이 필요한 자원. 절대 Room을 참조해서는 안 된다.
     *
     *  1) State는 Runnable을 구현하고, 그 안의 run()는  cleanable에 의해 딱 한 번만 호출될 것이다.
     *  2) cleanable 객체는 Room 생성자에서 cleaner에 Room과 State를 등록할 때 얻는다.
     *
     *  3) run() 메서드 호출되는 상황 2가지
     *   #1 Room의 close()를 호출할 때 :close()에서 Cleanable의 clean()을 호출하면, 이 메서드 안에서 run()를 호출한다.
     *   #2 GC가 Room을 회수할 때까지 클라이언트가 close를 호출하지 않는다면, cleaner가 State의 run()를 호출한다.
     */
    private static class State implements Runnable {
        int numJunkPiles; // 방 안의 쓰레기 수

        State(int numJunkPiles) {
            this.numJunkPiles = numJunkPiles;
        }

        // colse() 메소드가 호출되거나, Cleaner가 State의 run() 메소드를 호출해줄 것이다.
        @Override
        public void run() {
            System.out.println("Cleaning room");
            numJunkPiles = 0; // 방 청소 작업 수행
        }
    }

    // 방의 상태,  cleanable과 공유한다.
    private final State state;

    // cleanable 객체. 수거 대상이 되면 방을 청소한다.
    private final Cleaner.Cleanable cleanable;

    // Room 생성자
    public Room(int numJunkPiles) {
        // 초기 상태 설정
        state = new State(numJunkPiles);
        // cleaner에 Room과 State를 등록하여 cleanable을 얻는다.
        cleanable = cleaner.register(this, state);
    }

    // AutoClseable 인터페이스를 구현한 close() 메서드를 오버라이드
    @Override
    public void close() {
        // close() 메서드 호출 시, 청소 작업을 수행한다.
        cleanable.clean();
    }
}
```

위의 코드의 주요 내용은 다음과 같다.
+ `1` Room 클래스는 AutoCloseable 인터페이스를 구현하고 있다. <br> 이는 try-with-resources 구문에서 자동으로 리소스를 정리하기 위해 사용된다.
+ `2` Cleaner 클래스는 Java 9부터 도입된 것으로, 객체가 더 이상 참조되지 않을 때 미리 정의된 작업을 실행할 수 있게 해준다. <br> 이 코드에서는 cleaner 객체를 활용하여 자원 정리를 수행한다.
+ `3` Room 클래스 내부에 State 클래스가 정의되어 있다.  <br> 이 클래스는 방의 상태를 나타내며, Runnable 인터페이스를 구현하여 run() 메소드를 갖고 있다.
+ `4` Room 클래스의 생성자에서는 State 객체와 해당 객체를 정리하는 cleanable을 초기화하고, cleaner에 등록한다.
+ `5` close() 메소드가 호출되면 cleanable.clean()을 통해 정리 작업이 수행된다.  <br> 이는 try-with-resources 구문이나 명시적인 close 호출에 의해 실행된다.
+ `6` State 클래스의 run() 메소드는 방을 청소하는 작업을 수행한다. <br> 이 메소드는 cleaner에 의해 호출되며, 방이 더 이상 사용되지 않을 때 자동으로 실행될 수 있다.


### 잘 짜인 클라인트 코드의 예 : try-with-resources 구문
위 코드는 안전망을 만들었을 뿐이다. 클라이언트가 try-with-resources 블록으로 감쌌다면 방 청소를 정상적으로 출력한다.
```java
package study.effectivejava.item8;

import ch.qos.logback.core.encoder.JsonEscapeUtil;

public class Adult {
    public static void main(String[] args) {
        try (Room myRoom = new Room(7)) {
            System.out.println("Goodbye");
        }
    }
}
```
```
Goodbye
Cleaning room  # 방 청소를 출력한다. (청소됨)
```

### 실패한 클라이언트 코드의 예 : 예측할 수 없다.
주어진 코드는 Room 객체를 생성하고 "Peace out"을 출력한 후, 아무런 정리 작업 없이 프로그램이 종료되는 형태다. <br>
이 코드의 주된 문제는 자원 해제가 명시적으로 이루어지지 않는다는 것이다.  <br>
Room 클래스가 AutoCloseable을 구현했기 때문에, 명시적으로 close() 메소드를 호출하거나 try-with-resources 구문을 사용하여 자원을 안전하게 해제해주어야 한다.
```java
package study.effectivejava.item8;

public class Teenager {
    public static void main(String[] args) {
        new Room(99);
        System.out.println("Peace out");

        //System.gc();
    }
}
```
```
Peace out  #(방 청소를 하지 않는다.)
```



+ 자바 프로그램은 JVM 위에서 실행된다. (운영체제에서 바로 실행되지 않는다.)
+ 따라서 운영체제의 모든 기능을 직접 이용하기는 어렵다.
+ java.lang 패키지에 속하는 System클래스를 이용하면, 운영체제의 일부 기능을 이용할 수 있다.
+ 즉, 프로그램 종료, 키보드로부터 입력, 모니터로 출력, 현재 시간 읽기 등이 가능하다. 
+ System클래스의 모든 필드와 메소드는 정적 필드와 정적 메소드로 구성되어 있다.

## 프로그램 종료 exit()
+ 경우에 따라서는, 강제적으로 JVM을 종료시킬 때도 있다.
+ 이때 System 클래스의 exit() 메소드를 호출하면 된다. (:프로세스를 강제 종료시키는 역할)
+ exit()는 int매개값을 지정할 수 있는데, 이 값이 '종료 상태값'이다.
+ 일반적으로 정상종료일 경우, System.exit(0)을 준다.

### 예제 
+ System.exit(0)은 강제종료하므로 마지막 print()문은 출력되지 않는다.
+ 만약 print()를 출력하려면, exit()를 주석하고 break를 걸어야 한다.
```java
public class Main {
    public static void main(String[] args) {
        for (int i=0; i<10; i++) {
            if (i==5) {
                System.exit(0);
                //break();
            }
        }
        System.out.println("마무리");
    }
}
```

## 현재 시각 읽기 currentTimeMillis() nanoTime()
+ 현재 시간을 읽어서 밀리세컨드(1/1000초) 단위와 나노세컨드(1/10^9)단위의 long값을 리턴한다.

### 예제: 프로그램 실행 소요 시간 구하기
for() 문을 사용해서 1~10000000까지의 합을 구하는 데 걸린 시간을 출력하자. <br>

```java
public class Main {
    public static void main(String[] args) {
        long time1 = System.nanoTime(); //시작 시간 읽기

        int sum = 0;
        for (int i=1; i<=10000000; i++) {
            sum += i;
        }

        long time2 = System.nanoTime(); //끝 시간 읽기

        System.out.println("1~10000000까지의 합: " + sum);
        System.out.println("시간소요(나노초): " + (time2 - time1));
    }
}
```

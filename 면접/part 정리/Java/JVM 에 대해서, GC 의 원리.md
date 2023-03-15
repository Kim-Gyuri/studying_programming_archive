# 📌 목차
+ JVM
  + JVM이란?
  + 왜 자바 가상머신을 알아야 하는가?
  + 자바 프로그램 실행과정
  + JVM 구성
     + Class Loader(클래스 로더)
     + Excetion Engine(실행 엔진)
     + Interpreter (인터프리터)
     + JIT (Just - In - Time)
     + Garbage Collector
  + Runtime Data Area
     +  static 키워드가 있는 경우 로딩하는 방법
     +  static 키워드가 없는 경우 로딩하는 방법
+ GC의 원리
  + Java의 가비지 컬렉터(Garbage Collector) 동작원리
  + 힙 영역에 있는 객체에 대한 참조 

## JVM
### JVM이란?
+ JAVA Virtual Machine
+ 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행하는 것이다.
+ JAVA와 OS 사이에서 중개자 역할을 수행한다.
+ JAVA가 OS에 구애받지 않고 재사용을 가능하게 해준다.
+ 메모리 관리, Garbage Collection을 수행한다.
+ Stack 기반의 가상머신이다.

<br>

###  왜 자바 가상머신을 알아야 하는가?
+ 메모리 관리를 목적
+ 메모리 관리가 되지 않은 경우, "속도저하/튕김" 현상이 일어날 수 있다.

<br>

### 자바 프로그램 실행과정
프로그램이 실행되면, JVM은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당 받는다. <br> JVM은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.<br> <br>

![자바 프로그램 실행과정](https://user-images.githubusercontent.com/57389368/225216345-0bf161f4-c58b-4c36-948a-5a3dbb43d941.png) <br>
+ `1` 자바 컴파일러(javac)가 자바 소스(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.
+ `2` class Loader를 통해 class 파일들을 JVM으로 로딩한다.
+ `3` 로딩된 class 파일들은 Execution engine을 통해 해석된다.
+ `4` 해석된 바이트 코드는 Runtime Data Areas 에 배치되어 실질적인 수행이 이루어지게 된다.

이러한 실행과정 속에서 JVM은 필요에 따라 Thread Synchronization과 GC 같은 관리작업을 수행한다.

<br>

### JVM 구성
#### Class Loader(클래스 로더)
자바는 동적으로 클래스를 읽기어오기 때문에 (컴파일 타임이 아니라) 프로그램이 실행 중인 런타임에서 JVM과 연결된다. <br>
메모리에 필요한 해당 클래스(.class)를 loading 하고, link를 통해 배치하는 작업을 수행하는 모듈이다. <br>

#### Excetion Engine(실행 엔진)
클래스를 실행시키는 역할 <br>
클래스 로더가 JVM 내 런타임 데이터 영역에 바이트 코들 배치 시킨 것을 실행 시킨다. <br>

#### Interpreter (인터프리터)
실행 엔진이 자바 바이트 코드를 명령어 단위로 읽어서 실행한다.

#### JIT (Just - In - Time)
인터프리터 방식 단점을 보완하기 위해 도입된 컴파일러다. <br>
실행시점에 인터프리터 방식으로 기계어 코드를 생성할 때 (자주 사용되는) 메소드의 경우 컴파일하고 기계어를 (캐싱)한다. <br>
그리고 해당 메소드가 여러 번 호출할 때 매번 해석하는 것을 방지한다. <br>
 
#### Garbage Collector
GC를 수행하는 모듈(thread)가 있다.

## Runtime Data Area
프로그램 수행을 위해 OS에 할당된 메모리 공간
> 동작할 때 필요한 메모리 영역 : method Area, Stack Area, Heap Area, Literal Pool
#### PC Register
Thread가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로 thread마다 1개씩 존재한다. <br>
Thread가 어떤 부분을 더떤 명령으로 실행해야 하는지 기록한다.  <br>
(현재 수행 중인 JVM 명령 주소를 갖는다.) <br>

<br>

#### Stack Area
프로그램 실행과정에서 할당했다가 메소드가 빠져 나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다. <br> 메서드의  정보를 저장

<br>

#### method Area
method의 byte code가 저장되는 영역을 말한다.
> static zone, non static zone으로 나누어진다. <br>
> "클래스 정보"를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간이다. <br>
>(자바 프로그램은 main 메소드의 호출에서부터 계속된 메소드 호출로 흐름을 이어간다.) <br>
>(대부분 인스턴스 생성도 메소드 내에서 명령하고 호출한다.) <br>
>Runtime Constant Pool은 상수 자료형을 저장하여 참조하고 중복을 막는다. <br>
> <br>
> 올라가는 정보 <br>
> Field Info : 멤버변수 이름, 데이터 타입, 접근 제어자에 대한 정보 <br>
> Method Info : 메소드의 이름, 리턴타입, 매개변수, 접근제어자 정보 <br>
> TyPe Info : 클래스인지 인터페이스인지 타입 정보 <br>

<br>

#### Native Method Area
자바가 아닌 다른 언어로 작성된 코드를 바이트 코드로 전환하여 저장한다.

<br>

### static 키워드가 있는 경우 로딩하는 방법
> `예시`
```java
Class Solution {
    public static void main(String[] args) {
        int a=10; int b=20;
        int v = add(a,b);
        System.out.println(v);
    }

    private static int add(int a, int b) {
        return a+b;
    }
}
```

![static 키워드가 있는 경우](https://user-images.githubusercontent.com/57389368/225220480-7a5842b6-745f-4a36-b5ab-ade0117b2e22.png) <br>
`1` 해당 클래스를 현재 디렉토리에서 찾는다. <br>
> 성공 (대부분 성공한다.) <br>
> 실패 -> classpath 변수에 폴더를 연결해준다. <br>

<br>

`2` 찾으면 클래스 내부에 있는 static 키워드가 있는 메서드를 메모리로 로딩한다. <br>
> static 키워드가 있는 메서드를 method Area의 static zone에 로딩한다. <br>
> " static zone에 main(), add()가 로딩된다. 그리고 TPC08 라벨이 연결된다.

<br>

`3` static zone에서 main() 메소드를 실행한다. (호출시작) <br> main() 메소드가 호출되면, main() 메소드 호출정보가 stack Area에 들어간다.
> PC가 Stack 맨 밑바닥을 가르켰다가, main() 호출되면  main()을 가르킨다. <br>
> PC가 가르킨 위치가 "현재 프로그램이 동작되는 위치"이다.  <br>
> (그래서 stack을 "call stack frame Area"라고 한다.) <br>
> <br>
> 메소드가 호출될 때, stack frame(그 메서드를 위한 공간)에서 변수가 만들어진다. =  Local(지역) 변수 <br>
> 실제 프로그램에서는 main() 메소드가 local이다. <br>

<br>

`4` main()에서 add()를 호출한다. <br>
> method Area static zone에서 add()를 찾아야 한다. (없으면 에러발생) <br>
> "add()메소드도 static 키워드가 있어야 메모리에 올라올 수 있다." <br>
> `static 키워드가 없으면 에러가 나는 경우` <br>
>static이 아니기 때문에 static zone에 올라올 수 없어서 에러가 난다. <br>
>프로그램 실행 전에 자동로딩하기 위한 키워드다. 

<br>

`5` 그러면 add() 메서드도 호출되면, stack에 저장된다. <br> PC가 add()를 가르키고, "add()가 동작중이다" 알 수 있다.

<br>

`6` 순서상 add()가 종료되면, main()으로 내려온다. <br> 다시 제어권이 main()으로 돌아오면, main()이 동작하게 된다.

<br>

### static 키워드가 없는 경우 로딩하는 방법
객체를 생성하는 부분이 추가된다.
> heap Area: 객체 생성 영역 메모리, new 연산자 가 추가된다.

> `예시`
```java
Class Solution {
    public static void main(String[] args) {
        int a=10; int b=20;
        
        Solution sol = new Solution();
        int v = sol.add(a,b);
        System.out.println(v);
    }

    public int add(int a, int b) {
        return a+b;
    }
}
```

![static 키워드가 없는 경우, 로딩하는 방법](https://user-images.githubusercontent.com/57389368/225221103-58b54469-c7dd-474b-afca-375149cb0813.png) <br><br>


+ 1~3 단계까지 똑같다.

<br>

#### 근데 add() 메소드를 호출하기 위해 어떻게 메모리에 올릴 수 있을까?
객체를 생성한다.  <br>
```
TPC08 tpc = new TPC08();
int v = tpc.add(a,b); 
```

+ 객체를 생성하면 heap Area에 객체가 올라온다.
+ TPC08의  관련 메서드 add가 잡힌다.
+ 그 다음, method area non-static zone에 올라온다. (heap area와 포인터로 연결)
+ tpc.add()를 실행하면 stack Area에 메서드 정보다 저장된다.


## GC의 원리
> Method Area, Heap은 GC의 관리 대상에 포함된다.

### Java의 가비지 컬렉터(Garbage Collector) 동작원리
+ 힙 내의 객체 중에서 가비지를 찾아내고 찾아낸 가비지를 처리해서 힙이 메모리를 회수한다. 
+ 참조되고 있지 않은 객체를 가비지라고 한다.  <br>

#### reachability 개념으로 해당 객체가 가비지인지 판단한다 
>유효한 참조가 있다 -> reachability  <br>
>유효한 참조가 없다 -> unreachability

<br>

한 객체는 여러 다른 객체를 참조하고, 참조된 다른 객체들도 마찬가지로 또 다른 객체들을 참조할 수 있다. (참조사슬을 이룬다.) <br>
이런 상황에서 유효한 참조 여부를 파악하려면 항상 유효한 최초의 참조가 있어야 한다. ( 최초에 참조한 것을 Root set이라고 한다.) <br>

<br>

### 힙 영역에 있는 객체에 대한 참조 
> `JVM에서 메모리 영역인 런타임 데이터 영역 구조`<br> <br>스레드, 힙(객체를 생성/보관), 메서드(클래스 정보)로 나뉜다.  <br><br>
> ![Runtime Data Area 구조](https://user-images.githubusercontent.com/57389368/225226217-20bd5797-0628-4a2b-8e59-8e8671ec9fdf.png) <br>


<br>

객체에 대한 참조는 화살표로 표시되어 있다. <br> 힙 영역에 있는 객체에 대한 참조는 4가지 종류 중 하나이다.
1. 힙 내의 다른 객체에 의한 참조 
2. JAVA Stack (JAVA METHOD 실행 시에 사용하는 지여견수와 파라미터들에 의한 참조)
3. NATIVE STACK(자바 네이티브 인터페이스에 의해 생성된 객체에 대한 참조)
4. 메서드 영역의 정적 변수에 의한 참조
> 2~4 방법는 root set으로 rachability를 판단하는 기준이다.

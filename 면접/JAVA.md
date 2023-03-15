# 📌 목차
+ JVM 에 대해서, GC 의 원리
+ 
+ Generic
+ Annotation
+ Overriding vs Overloading
+ 접근 제한자(Access Modifier)
+ Wrapper class
+ final keyword

## [JVM 에 대해서, GC 의 원리](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EB%A9%B4%EC%A0%91/part%20%EC%A0%95%EB%A6%AC/Java/JVM%20%EC%97%90%20%EB%8C%80%ED%95%B4%EC%84%9C%2C%20GC%20%EC%9D%98%20%EC%9B%90%EB%A6%AC.md)
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

## [Collection](https://github.com/Kim-Gyuri/studying_programming_archive/blob/main/%EC%9E%90%EB%B0%94/%EC%BB%AC%EB%A0%89%EC%85%98.md)
+ List
+ Set
+ Map
+ Stack과 Queue

## Generic
클래스 내부에 타입을 적어두고, 컴파일 과정에서 타입체크를 해준다.
> 컴파일 때 해당 타입으로 캐스팅한다는 뜻 <br>

<br>

제네릭으로 선언한 클래스는, 내가 원하는 타입으로 만들어 사용이 가능하다. <br>
안에는 참조자료형(클래스, 인터페이스, 배열)만 가능하다. <br>
기본자료형을 이용하기 위해선 wrapper Typer으로 쓴다.

> `참고자료` <br>
> [Generic 참고할 글](https://st-lab.tistory.com/153)

## 어노테이션
프로그램에게 추가적인 정보를 제공하기 위한 것이다.

#### 어노테이션의 용도
1.컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공한다.
2.개발툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공한다.
3.실행시 특정 기능 실행에 정보를 제공한다.


#### 어노테이션 사용 순서
1. 어노테이션 정의 <br> 어노테이션을 적용할 때는 어노테이션이 어디에 적용되며 언제까지 어노테이션 소스가 유지 될 것인지 설정해야 한다.
2. 클래스에 어노테이션 배치
3. 코드가 실행되는 중에 Reflection을 이용하여 추가정보를 획득하여 기능 실시

> `참고자료` <br>
> [자바에서 어노테이션이란](https://honeyinfo7.tistory.com/56) <br>
> [스프링에 자주 쓰이는 어노테이션](https://melonicedlatte.com/2021/07/18/182600.html)


##  Overriding vs Overloading
> (객체지향을 키워드로 둔 질문) <br>
> (OPP 중 '다향성'와 관련된 질문이다.) <br>

#### 오버로딩 
메소드의 이름은 같고, 매개변수를 다르게 함으로써 여러 메소드를 만드는 것

#### 오버라이딩 
+ 부모클래스로부터 상속받은 메소드를 재정의하는 것. 
+ 자식 객체에서 오버라이딩한 메소드는 호출시 오버라이딩한 메소드가 우선시 되어 호출된다.
+ 상속 개념이 들어 갔으며 객체지향 "다형성"과 관련이 있다.
+ (동일한 리턴타입, 메소드 이름, 매개변수를 가져야함)

##  접근 제한자(Access Modifier)
> 변수 또는 메소드의 접근 범위를 설정해준다.

#### public 
모든 클래스 접근 가능

#### protected
클래스가 정의되어 있는 해당 패키지 내, 해당 클래스를 상속받는 외부 패키지의 클래스 접근 가능

#### default
클래스가 정의되어 있는 해당 패키지 내에 접근 가능

#### private
정의된 해당 클래스에만 접근 가능

## Wrapper class
+ `기본자료형`을 `객체 자료형`으로 사용할 수 있도록 만들어 놓은 자료형 (포장 클래스) 
+ 일단 컬렉션에서 제네릭을 사용하기 위해서 사용한다.

```
기본자료형        객체 자료형
int             -> new Integer();
float           -> Float
char            -> Character
boolean         -> Boolean
```

#### 왜 객체로 만들어야 하나?
Object와 상속관계로는 "클래스"만 가능하다.
```
 int <-> Object     (불가능)
 Integer <-> Object  (가능)
```


#### 변수에 1를 저장하는 방법
```java
1. int a = 1;
2. Integer b = new Integer(1); -> int a2 = b.intValue();
```

+ `1` 정수형 int 자료형
+ `2` new 생성을 하여 "객체"를 만들고, Integer의 메소드(intValue(), toString(), parseInt())를 사용할 수 있다. <br>
 메소드로 포장을 벋긴다. (정수로 꺼내기 intValue(), 문자로 가져오기:toString())

#### 기본 자료형을 Object[] 배열에 저장할 경우?
```java
Object[] obj = new Object[3];
obj[0] = 1;
obj[1] = new Integer(1);
obj[2] = new Integer(2);
(-> 이런 방식이 자바 정식 방식이다.)


Object[] obj = new Object[2];
obj[0] = 1;
obj[1] = 2;
(-> 그런데, 이렇게 해도 에러가 없다.)
```

> `설명`
```
 Integer a = 1;            (Boxing) # 이 경우, 컴파일러가 자동으로 "1" 부분을 객체로 자료형으로 만들어준다.
 int b = new Integer(10);  (unboxing: 포장을 벗긴다.) 
```

#### 예시
```java
Integer b1 = new Integer(1);  # 이렇게 하면, deprecated 되었다고 알려준다. (사실 구버전임)
Integer b2 = 1;               # 이렇게 해도 컴파일러가 Boxing 해준다.


Object[] obj = new Object[2];
obj[0] = 1;        # 부모 = 자식
obj[1] = 2;
System.out.println(obj[0].toString());



# 컬렉션
List<Integer> list = new ArrayList<>();
list.add(1);     # 여기서 list.add(Integer(1)); 이렇게 안해도 컴파일러가 Boxing 해준다.
```

## final keyword
#### final class
다른 클래스에서 상속하지 못한다.

#### final method
다른 메서드에서 오버라이딩하지 못한다.

#### final variable
변하지 않는 상수값을 정의

> `혼동되는` <br>
> `1` finally <br>
> try-catch 구문이다, 정상적 작업처리와 에러처리를 포함하여 작성하는 code block <br>
> <br>
> `2` finalize() <br>
> keyword도 아니고 code block도 아닌 method다. <br>
> GC에 의해 호출되는 함수로 절대 호출해서는 안 되는 함수다. (해당 메소드가 실행된다는 보장이 없다.)


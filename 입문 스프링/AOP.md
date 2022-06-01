+ 모든 메소드의 호출 시간을 측정하고 싶다면?
+ 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern)
+ 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면?

![임문5](https://user-images.githubusercontent.com/57389368/171468384-e892b134-79a7-4fbb-a718-afdcd271e1bc.JPG)

---
## MemberService 회원 조회 시간 측정 추가하기
```java
long finish = System.currentTimeMillis();
 long timeMs = finish - start;
 System.out.println("findMembers " + timeMs + "ms");
```

### 문제점
+ 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다.
+ 시간을 측정하는 로직은 공통 관심 사항이다.
+ 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다.
+ 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다.
+ 시간을 측정하는 로직을 변경할 때 모든 로직을 찾아가면서 변경해야 한다.

---
## AOP 적용
+ AOP: Aspect Oriented Programming
+ 공통 관심 사항(cross-cutting concern) vs 핵심 관심 사항(core concern) 분리

![입문6](https://user-images.githubusercontent.com/57389368/171469017-bd6d2ab2-4b92-460d-b83e-c7c8195e9e23.JPG)
+ 시간 측정로직을 한 군데에 넣어, 원하는 곳에 공통관심 사항을 적용한다.

---
## 시간 측정 AOP 등록하기
+ AOP폴더를 만들어 TimeTraceAop을 만든다.
```java
 @Around("execution(* hello.hellospring..*(..))") //패키지명 지정하기
 public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
    long start = System.currentTimeMillis();
    System.out.println("START: " + joinPoint.toString()); //start 콜시간 찍기
    try {
      return joinPoint.proceed();
    } finally {
      long finish = System.currentTimeMillis();
      long timeMs = finish - start;
      System.out.println("END: " + joinPoint.toString()+ " " + timeMs + "ms");
    }
 }
```

+ 스프링빈 등록하기 <br> springConfig에 직접 등록하거나 <br> TimeTraceAop에 @Component을 추가한다. 

<br>

---
### AOP로 해결한 것
+ 회원가입, 회원 조회등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리한다.
+ 시간을 측정하는 로직을 별도의 공통 로직으로 만들었다.
+ 핵심 관심 사항을 깔끔하게 유지할 수 있다.
+ 변경이 필요하면 이 로직만 변경하면 된다.
+ 원하는 적용 대상을 선택할 수 있다.

---
## AOP 적용 후 의존관계
![입문8](https://user-images.githubusercontent.com/57389368/171470371-8f88bfc0-5f1b-41fb-838d-960ad692ee95.JPG)
+ `스프링빈 등록할 때, <br> 가짜(프록시) mmemberService를 앞에 세워 두고, 가짜 memberService가 끝나면 진짜 memberService를 호출한다.`
+ 프록시는 memberService.getClass();를 찍어보면 볼 수 있다, <br> MemberService...EnhancerBySpring(GLIB...) 확인가능

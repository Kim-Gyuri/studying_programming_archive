## 스프링 데이터 접근 기술
+ JDBC (초기)
+ 스프링 JdbcTemplate (초기에서 +발전)
+ JPA  (초기+발전+발전)
+ 스프링 데이터 JPA (JPA를 쉽게 관리함)

---
##  `1` 순수 Jdbc
+ `1` 환경 설정
> `build.gradle 파일에 jdbc, h2 데이터베이스 관련 라이브러리 추가` <br> implementation 'org.springframework.boot:spring-boot-starter-jdbc' <br> runtimeOnly 'com.h2database:h2'


+ `2` 스프링 부트 데이터베이스 연결 설정 추가
> ` resources/application.properties에 설정 추가하기` <br> spring.datasource.url=jdbc:h2:tcp://localhost/~/test <br> spring.datasource.driver-class-name=org.h2.Driver <br> spring.datasource.username=sa

+ `3` JdbcMemberRepository 생성하기
> DB에 북기 위해 필요한 DataSource를 추가한다. <br> 

+ `4` 스프링 설정 변경하기
> @Bean <br>
 public MemberRepository memberRepository() { <br>
// return new MemoryMemberRepository();  <br>
return new JdbcMemberRepository(dataSource); //<----JdbcMemberRepository로 바꾸기 <br>
} 

<br>

+ ` DataSource는 데이터베이스 커넥션을 획득할 때 사용하는 객체다.` <br> `스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 DataSource를 생성하고 스프링 빈으로 만들어둔다.` <br> `그래서 DI를 받을 수 있다.` 


> `참고` <br> 과거에는 필요한 서비스 코드를 수정해야 했고 <br> 현재는 애플리케이션 조립코드만 수정하면 됨

---
## 구현클래스 추가 이미지
![입문3](https://user-images.githubusercontent.com/57389368/171458347-9cb6fdda-0000-49e4-9ffd-3f53a9cfe128.JPG)
+ MemberService는 interface(MemberRepository)를 의존한다.
+ interface 구현체로 MemoryMemberRepository, JdbcMemberRepository가 있다.

---
## 스프링 설정 이미지
![임문4](https://user-images.githubusercontent.com/57389368/171458696-bc2ef316-fc1d-4bf8-8cac-f9cbcc636db7.JPG)
+ 컨트롤러에서 해당 서비스를 구현한다.
+ 메모리 코드 수정없이 , 설정만으로 구현 클래스 바꾸기 가능

> `개방-폐쇄 원칙(OCP, Open-Closed Principle)` <br> 
확장에는 열려있고, 수정, 변경에는 닫혀있다. <br>
스프링의 DI (Dependencies Injection)을 사용하면 기존 코드를 전혀 손대지 않고, 설정만으로 구현 클래스를 변경할 수 있다. <br>
회원을 등록하고 DB에 결과가 잘 입력되는지 확인하자.<br>
데이터를 DB에 저장하므로 스프링 서버를 다시 실행해도 데이터가 안전하게 저장된다. <br>

---
## 스프링 통합 테스트
+ @SpringBootTest : <br> 스프링 컨테이너와 테스트를 함께 실행한다.
+ @Transactional : <br> 테스트 케이스에 이 애노테이션이 있으면, 테스트 시작 전에 트랜잭션을 시작하고, <br>
테스트 완료 후에 항상 롤백한다. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.


---
## `2` 스프링 JdbcTemplate
+ 스프링 JdbcTemplate과 MyBatis 같은 라이브러리는 JDBC API에서 본 반복 코드를 대부분 제거해준다.
+ 하지만 SQL은 직접 작성해야 한다.

---
## JdbcTemplateMemberRepository 만들기
+ 구현체므로 해당 인터페이스 implement하기
> implements MemberRepository
+ JdbcTemplate 회원 리포지토리를 추가한다.
> private final JdbcTemplate jdbcTemplate;
+ 생성자 주입하기
>  //@Autowired 생략 가능 <br>
>  public JdbcTemplateMemberRepository(DataSource dataSource) { <br>
 jdbcTemplate = new JdbcTemplate(dataSource); <br>
 } <br>

+ 조회 쿼리 추가하기
+ ` 전체적으로 보면' <br> `save --> id로 보면 --> jdbcTemplate --> RowMapping --> List<>로 받기 --> 객체  반환` 


---
## `3` JPA
+ `2` 스프링 JdbcTemplate : JDBC에 비해 반복코드를 줄임, SQL은 직접 작성필요
+ `3` JPA : SQL quert도 자동처리 해줌
> `JPA는, 객체를 메모리에 넣듯이 JPA에 넣으면 DB에 SQL을 날리고 dataDB를 가지고 하는 걸 다 처리해준다.` <br>

+ JPA는 `객체중심` 설계로 패러다임 전환가능하고, 개발 생산성을 높일 수 있다.
> Google Trends에서 보면 JPA와 마이바티스 비교 시, 2015년부터 JPA가 상승했다.

+ build.gradle 파일에 JPA, h2 데이터베이스 관련 라이브러리 추가하기
+ 스프링 부트(resources/application.properties)에 JPA 설정 추가하기
+ JPA 엔티티 매핑
+ JPA 회원 리포지토리
+ 서비스 계층에 트랜잭션 추가

---
## `4` 스프링 데이터 JPA
+ `스프링 데이터 JPA가 해당 구현체를 스프링 빈으로 자동 등록해준다`
+ 스프링부트와 JPA만 사용해도 개발 생산성이 좋아진다.
+ 코드도 줄어듬
+ 여기에 스프링 데이터 JPA를 사용하면 기존의 한계를 넘어, 리포지토리에 구현 클래스없이 인터페이스만으로 개발가능
+ 기본 CRUD기능도 제공해줌
> 메소드 이름만으로 조회기능 제공 ex: findByName(), findByEmail() <br> 페이징 기능도 

<br>

> `참고사항` <br> 실무에서는 JPA와 스프링 데이터 JPA를 기본으로 사용하고, <br> 복잡한 동작쿼리는 QueryDSL 라이브러리를 사용한다. <br> QueryDSL을 사용하면 쿼리도 자바코드를 안전하게 작성가능하다. <br>


<br>

+ 스프링 데이터 JPA 회원 리포지토리 만들기
+ SpringConfig에  MemberRepository 등록하기
> 이렇게 하면, 스프링 컨테이너에서 MemberRepository를 찾는다.


<br> <br>

# 정리
+ JDBC (초기)
+ 스프링 JdbcTemplate (초기에서 +발전) : 직접 쿼리작성필요
+ JPA  (초기+발전+발전) :통합 테스트 가능 :쿼리작성필요X, select같은 기능은 JPAQueryDSL 필요함
+ 스프링 데이터 JPA (JPA를 쉽게 관리함) : 구현클래스 작성 필요없이 인터페이스로 개발 끝냄 

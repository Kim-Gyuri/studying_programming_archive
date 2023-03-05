# 📌 개발상식
+ REST과 RESTful이란?
+ REST 6 가지 원칙
  + Uniform Interface
  + Stateless
  + Caching
  + Client-Server
  + Hierarchical system
  + Code on demand
+ Uniform Interface을 자세히 정리한 것
  + REST API란?
  + 자원에 대한 식별 (identification of resources)
  + 표현을 통한 자원에 대한 조작
  + 서술적 메시지 (self-descriptive messages)
  + HATEOAS (hypermedia as the engine of application state)
  + 꼭 REST API 이여야 하는 걸까?
  + API 설계의 방향성
+ RESTful하게 API를 디자인 한다는 것은 무엇을 의미하는가?
+ RESTful 장단점

## REST과 RESTful이란?
#### REST 
API 설계의 중심에 자원(Resource)이 있고 HTTP Method 를 통해 자원을 처리하도록 설계하는 것이다.

#### RESTful
REST 의 기본 원칙을 성실히 지킨 서비스 디자인은 'RESTful'하다라고 표현할 수 있다.

##  REST 6 가지 원칙
### Uniform Interface
REST는 HTTP 표준에만 따른다면, 어떠한 기술이라든지 사용이 가능한 인터페이스 스타일이다. <br>
> `예시` <br>
> HTTP + JSON으로 REST API를 정의했을 경우, <br> 언어 플랫폼 상관없이 HTTP와 JSON을 사용할 수 있는 모든 플랫폼에 사용이 가능하다.

<br>

### Stateless
+ Stateless (상태 유지하지 않는다) 
+ "상태가 있다거나 없다는 것" 는 "사용자나 클라이언트의 컨텍스트를 서버 쪽에 유지 하지 않는다"를 의미한다. 
+ HTTP Session과 같은 컨텍스트 저장소에 상태 정보를 저장하지 않는 형태를 의미한다. (구현이 단순해진다.)
+ 상태 정보를 저장하지 않으면, 각 API 서버는 (들어오는) 요청을 (들어오는) 메시지로만 처리한다.

<br>

### Caching
+ HTTP라는 기존의 웹 표준을 그대로 사용한다. 
+ 캐쉬를 사용하게 되면 네트웍 응답시간뿐만 아니라, REST 컴포넌트가 위치한 서버에 트렌젝션을 발생시키지 않기 때문에, <br> 전체 응답시간과 성능 그리고 서버의 자원 사용률을 비약적으로 향상할 수 있다.

> `웹에서 사용하는 기존의 인프라를 그대로 활용 가능하다.` <br>
> HTTP 프로토콜 기반의 로드 밸런서, SSL <br>
> HTTP가 가진 강렬한 특징 중 하나가 Cashing 기능. <br>
> <br>
> `장점` <br>
> 일반적인 서비스 시스템에서 60~80% 가량의 트랜젝션이 Select 같은 조회성 트랜젝션인 것을 감안하면, <br>
> HTTP의 리소스들을 웹캐시 서버에 캐쉬하는 것은, 용량이나 성능 면에서 많은 장점이 된다. <br>
> <br>
> `구현` <br>
> HTTP 프로토콜 표준에서 사용하는 "Last-Modified" 태그나 "E-Tag"를 이용하면 Cashing을 구현할 수 있다. <br>
> <br><br>
> `예시` <br>
> "Last Modified 필드를 이용한 Cashing 처리방식" 
> (1) 첫번째 요청, <br>
>   Client가 HTTP GET 요청을 보낼 때, <br>
>   검증 헤더(Last-Modified)를 추가했다. (데이터가 마지막에 수정된 시간을 포함해서 전송한다.) <br>
>   받은 응답을 브라우저 캐시에 저장한다. <br>
> <br>
>  (2) 두번째 요청을 보낸다. (캐시 시간이 초과된 경우) <br>
>   GET 요청했을 때, 먼저 브라우저 캐시를 조회한다. (캐시 유효시간이 초과된 경우) <br>
>   서버에 (최종수정일을 포함하여) 재요청한다. <br>
>   서버는 요청에 대해 검증한다.(수정된 것이 있는지 확인) <br>
>   서버가 Client에게 (HTTP헤더만) 응답을 전송한다.  <br>
>  서버는 (이때 수정된 것이 없다면) "304 Not Modified" 리턴, HTTP Body 없이 보낸다. <br>
>  다시 브라우저 캐시가 갱신된다. <br>
> <br>
> (3) 세번째 요청을 보낸다.<br>
> if-modified-since(조건부)를 포함한다.<br>
> (브라우저에서 해당 리소스를 재요청할 경우에는 서버에서 Last-Modified 헤더에 설정한 값을 If-Modified-Since 헤더에 포함시켜 서버에 요청합니다.)<br>
> 캐시 수정일을 판단 시 사용된다.<br>
> -> 데이터 미변경 경우 ( 캐시 == 서버 데이터), "304 Not Modified" 헤더 데이터만 보낸다.<br>
> -> 데이터 업데이트 경우 (캐시 != 서버 데이터), "200 ok" 모든 데이터(Header, Body 포함) 전송한다.

<br>

### Client-Server
+ 아키텍쳐를 단순화 시키고 더 작은 단위로 분리시킨다. 
+ Rest 서버는 API를 제공하고, 제공된 API를 이용해서 비지니스 로직 처리 및 저장을 책임진다.
+ 클라이언트의 경우, "사용자 인증" "컨텍스트(세션,로그인 정보)" 직접 관리하고 책임지는 구조로 역할이 나뉜다.
> `장점` <br> 역할을 확실하게 구분하면, 개발 관점에서 클라이언트와 서버에서 개발해야 할 내용이 명확해지고, 개발에 있어서 의존성이 줄어들게 된다.

<br>

### Hierachical system ( Layered System)
클라이언트는 보통 대상 서버에 직접 연결되었는지, 또는 중간 서버를 통해 연결되었는지 알 수 없다. <br>
> Layered System (계층형 구조)는, "클라이언트가 REST API 서버만 호출한다."를 말한다. <br>
> 그러나 서버는 다중 계층으로 구성될 수 있다. <br>
> 순수 비즈니스 로직을 수행하는 API와, 그 앞단에 사용자 인증, 암호화(SSL), 로드밸런싱  계층 구조를 만들 수 있다. <br>
> 이는 마이크로 서비스 구조의 API GateWay를 이용해서 구현 가능 <br>
> 단순한 기능 경우에는 HA Proxy나 Apache와 같은 Reverse Proxy를 이용해서 구현 가능. <br>

<br>

###  Code on demand
자바스크립트, 자바 애플릿의 제공을 통해 서버가 클라이언트가 실행시킬 수 있는 로직을 전송하여 기능을 확장시킬 수 있다.

## Uniform Interface을 자세히 정리한 것
#### REST API란?
REST 아키텍쳐 스타일에 부합하는 API -> Uniform Interface 초점에서 <br>
아래 4가지를 만족해야 REST API라고 부를 수 있다는 주장이다.

#### 1. 자원에 대한 식별 (identification of resources)
> 자원은 객체 <br>
> 객체는 시간에 따라 변화/파괴 되기도한다. <br>
> 그래서, 변하지 않은 식별자가 필요하다. : 식별자는 URI다.  <br>
> 예: user/1 <br>

<br>

#### 2. 표현을 통한 자원에 대한 조작  
(manipulation of resources through reresentations)

+ 표현: 특정한 상태의 자원에 대한 표현
+ 자원은 (예: 문서, 파일, HTTP 메시지 엔티티) 방식으로 표현 가능하다.

```
사용자 -> HTTP/1.0 200 ok content-type...

그래서, 클라이언트가 서버에 "GET /user/1 HTTP/1.1"을 요청하면
-> 자원을 문서로 현재 상태를 표현했다고 한다.

또한 이미지 형식으로 서버에게 "POST /image/이미지"을 요청한다면,
서버에서는 이미지 표현을 바탕으로 새로운 이미지 자원을 생성한다.
/image/1를 식별자로 할당한다.
# REST :자원에 대한 표현 전송한다.
REpresentational State Transfer  : 자원의 상태를 전송한다.
                                     # 자원의 상태: 자원의 현재 상태 또는 자원의 기대되는 상태

```

#### 3. 서술적 메시지 (self-descriptive messages)
`포인트` <br>
메시지는 스스로에 대해 충분한 설명이 있어야 한다. <br>
누구에게 설명하는 걸까? <br>

<br>

+ 클라이언트와 서버 사이의 컴포넌트들이 메시지들을 적절히 수행한다. <br>
(클라이언트가 서버에 도착하고,서버에서 클라이언트에게 응답할 때)

```
예시(1) : Host 헤더에 도메인명을 기재한다. 
(가상호스트 문제, 1개의 IP주소에 복수의 도메인명 존재 가능하다.)
(IP 주소만으로는 요청 대상을 찾아낼수 없다.)

예시(2) : 캐시 정보, 캐시 관련 헤더를 통한 캐쉬 전략 지정
HTTP/1.1부터는 Cache-Control,Age,Etag,Vary 통해 커스텀 가능
```

<br>

#### 4. HATEOAS (hypermedia as the engine of application state)
매번 URI 입력하는 방법은 까다롭다.
화면에 랜더링된 하이퍼미디어를 통해 페이지 이동가능하다.

#### 심화 : "HATEOAS 위배 사례"
숨겨진 경로가 존재한다면? <br>
이건 프론트엔트가 처리해야 하는 일인가? <br>
> 프론트엔트 <-> 백엔드 json형식을 주고 받는다. <br>
> 프론트엔드에서는 json을 활용해 화면을 구성한다. <br>
> 그런데 벡엔드에서 HATEOAS 위배한 경우가 기본적으로 많다. <br>
> 이때 "json에 URI 등 요청정보를 포함하여 클라이언트에게 전송하면 괜찮을 것이다" 라는 주장도 있다.

```
예:
"/email 또는 /images 에 대해 GET 요청을 보낼 수 있다" 요청정보를 json에 담아 보낸다면,
벡엔드에서 HATEOAS 위배되지 않을 것이라는 주장이다.

"href" : http://~/email",         
"action" : "GET"

"href" : https://~/images",
"action" : "GET"
```

<br>

#### 5. 꼭 REST API 이여야 하는 걸까?
상황에 따라 최적이 아닐 수 있다.  <br>
> REST API는 표준화된 형식으로 데이터 전달이 목적이다.

<br>

#### 6. API 설계의 방향성
3가지 선택지가 있다. <br>
> 1 진짜 REST API 만들기 <br>
> 2 REST 스타일의 API  <br>
> 3 다른 표준 API <br>

## RESTful하게 API를 디자인 한다는 것은 무엇을 의미하는가?
`1` 리소스와 행위를 명시적이고 직관적으로 분리한다.
> 리소스는 URI로 표현되는데 리소스가 가리키는 것은 명사로 표현되어야 한다. <br>
> 행위는 HTTP Method로 표현하고, 분명한 목적으로 사용한다. <br>
> 예: GET(조회) POST(생성) PUT(전체 수정) PATCH(일부 수정) DELETE9삭제) <br>

<br>

 `2` Message 는 Header 와 Body 를 명확하게 분리해서 사용한다.
> Entity 에 대한 내용은 body 에 담는다. <br>
> 애플리케이션 서버가 행동할 판단의 근거가 되는 컨트롤 정보인 API 버전 정보, 응답받고자 하는 MIME 타입 등은 header 에 담는다. <br>
> header 와 body 는 http header 와 http body 로 나눌 수도 있고, http body 에 들어가는 json 구조로 분리할 수도 있다. <br>

<br>

 `3` API 버전을 관리한다.
> 환경은 항상 변하기 때문에 API 의 signature 가 변경될 수도 있음에 유의하자. <br>
> 특정 API 를 변경할 때는 반드시 하위호환성을 보장해야 한다. <br>

<br>

 `4` 서버와 클라이언트가 같은 방식을 사용해서 요청하도록 한다.
> 브라우저는 form-data 형식의 submit으로 보내고, 서버에서는 json 형태로 보내는 식의 분리보다는 <br>
> 둘 다 같은 형식으로(하나로) 통일한다. <br>
> 다른 말로 표현하자면 URI가 플랫폼 중립적이어야 한다.


## RESTful 장단점
#### 장점
+ Open API 를 제공하기 쉽다
+ 멀티플랫폼 지원 및 연동이 용이하다.
+ 원하는 타입으로 데이터를 주고 받을 수 있다.
+기존 웹 인프라(HTTP)를 그대로 사용할 수 있다.

#### 단점
+ 사용할 수 있는 메소드가 한정적이다.
+ 분산환경에는 부적합하다.
+ HTTP 통신 모델에 대해서만 지원한다.


# 📌 키워드
+ HTTP 버전별 차이 (HTTP/2가 빠른 이유)
+ IPS와 Inline 구조
+ WAF와 Proxy 구조
+ SSL인증서는 어디에 설치되나?

## HTTP 버전별 차이 (HTTP/2가 빠른 이유)
HTTP/2와 HTTP/3는 (성능개선 초점) 더 효율적인 데이터 전송을 지원하기 위해  <br>
다중 스트림, 헤더 압축, 서버 푸시 등을 도입하여 웹 페이지 로딩 시간을 단축하고 빠른 웹 서비스를 제공합니다. <br>
이러한 향상된 기능들은 더 나은 사용자 경험과 빠른 웹 페이지 로딩을 가능하게 합니다.<br>
+ HTTP/1.1 : 가장 많이 사용, 대부분의 기능이 있다. (TCP 위에서 동작)
+ HTTP/2 : 성능개선 (TCP 위에서 동작)
+ HTTP/3 : TCP 대신에 UDP 기반으로 개발, 성능개선
> `HTTP/3은 UDP 기반인 이유?` <br>
> TCP는 3 way handshake도 해야 하고, 기본적으로 데이터가 많고, 메커니즘이 느린 속도다. <br>
> 그래서 UDP 위에 애플리케이션 레벨에서 성능 최적화하도록 나온게 버전 3이다. <br>
> 


### HTTP/1.1
#### 1. 후속 요청 지연
+ HTTP/1.1에서는 요청과 응답이 순차적으로 처리되어,  <br> 하나의 요청이 완전히 처리될 때까지 다음 요청을 기다려야 합니다.
+ 이로 인해 불필요한 대기 시간이 발생할 수 있습니다.
#### 2. 요청 병렬 처리
+ 브라우저는 동시에 여러 개의 TCP 연결을 열어 병렬 요청을 보냅니다. 
+ 이를 통해 일부 지연을 완화하지만 여전히 여러 개의 연결을 관리해야 합니다.
### HTTP/2
#### 1.  이진 프레임 형식
+ HTTP/2는 데이터를 이진 프레임 형식으로 전송하며,  <br>텍스트 기반의 형식을 사용하는 HTTP/1.1과 비교하여 더 효율적입니다.

#### 2. 헤더 압축 
+ HTTP/2에서는 헤더 필드를 압축하여 불필요한 데이터 전송을 줄입니다. 
+ 이로써 헤더의 크기가 큰 웹 페이지의 로딩 시간을 단축시킬 수 있습니다.

#### 3. 다중 스트림 
+ 하나의 TCP 연결을 통해 다중 스트림을 지원하여 여러 요청과 응답을 동시에 처리할 수 있습니다.
+ 이는 요청의 병렬 처리를 간편하게 만듭니다.

#### 4. 서버 푸시
+ 서버는 클라이언트의 요청 없이 리소스를 미리 보낼 수 있도록 "서버 푸시" 기능을 제공합니다. 
+ 이로 인해 불필요한 요청과 응답 사이의 대기 시간을 제거할 수 있습니다.
### HTTP/3
#### 1. UDP 기반 QUIC 프로토콜
+ HTTP/3는 기존의 TCP 대신에 UDP 기반의 QUIC(Quick UDP Internet Connections) 프로토콜을 사용합니다.
+ 이로써 연결 설정 및 종료에 대한 지연 시간을 줄이고, 더 빠른 데이터 전송을 가능하게 합니다.
#### 2. 흐름 제어 및 오류 복구
+ HTTP/3는 흐름 제어와 오류 복구 기능을 통해 신뢰성 있는 통신을 제공합니다. 
+ 연결이 끊겨도 데이터 손실을 최소화합니다.

## IPS와 Inline 구조
IPS (Intrusion Prevention System)와 Inline 구조는 네트워크 보안에 사용되는 두 가지 다른 시스템 구성을 나타냅니다. <br><br>
결론적으로, IPS는 네트워크의 보안을 향상시키는 역할을 하며, <br>
Inline 구조는 이러한 IPS 또는 방화벽과 같은 장비를 네트워크 트래픽 경로에 직접 배치하여 더 강력한 보안을 제공합니다. <br>
그러나 Inline 구조는 장애 시에 네트워크에 심각한 영향을 미칠 수 있으므로 신중하게 구성해야 합니다. 
### 1. IPS (Intrusion Prevention System)
- IPS는 네트워크 보안 시스템으로, 네트워크 트래픽을 모니터링하고 침입 시도를 탐지하며 차단하는 역할을 합니다.
- IPS는 탐지와 차단을 위한 기능을 포함하고 있어, 발견된 침입 시도나 악성 활동을 실시간으로 차단할 수 있습니다.
- 대개 네트워크 트래픽을 복제하거나 스니핑하여 탐지하며, 알려진 침입 시그니처와 비정상적인 동작 패턴을 검사하여 탐지합니다.
- 주로 내부 네트워크와 외부 네트워크 사이의 경계에 위치하며,  침입 탐지 시스템 (IDS)**과 비슷한 역할을 하지만 패킷을 차단하여 보안을 강화하는 차이가 있습니다.
### 2. Inline 구조
- Inline 구조는 IPS나 방화벽과 같은 보안 장비가 네트워크 트래픽의 실제 경로에 직접 포함되어 있는 구조를 나타냅니다.
- 즉, 네트워크 패킷이 이러한 장비를 거치지 않고는 네트워크를 통과할 수 없습니다.
- 이것은 더 높은 보안을 제공할 수 있지만, 장애 발생 시 네트워크에 영향을 미칠 수 있으므로 주의가 필요합니다.
- Inline 구조에서는 보안 장비가 패킷을 차단하거나 허용하기 전에 정교한 판단을 내릴 수 있습니다.

## WAF와 Proxy 구조
WAF (Web Application Firewall)와 Proxy 구조는 웹 보안과 네트워크 보안 분야에서 사용되는 두 가지 다른 시스템 구성을 나타냅니다.
### 1. WAF (Web Application Firewall)
- WAF는 웹 애플리케이션의 보안을 강화하는 장치 또는 소프트웨어입니다.
- 주로 웹 애플리케이션 레벨에서 보안 위협을 탐지하고 차단하는 역할을 합니다.
- 웹 애플리케이션 레이어에서 공격 시도를 모니터링하고, SQL 인젝션, 크로스 사이트 스크립팅 (XSS), CSRF (Cross-Site Request Forgery)와 같은 공격을 탐지하여 차단합니다.
- WAF는 주로 웹 서버 앞에 배치되며, 클라이언트와 웹 서버 사이에서 웹 트래픽을 검사하여 보안 위협을 차단합니다.
### 2. Proxy 구조
웹 프록시 (Web Proxy)는 HTTP/HTTPS 트래픽을 중개하며, 웹 콘텐츠 필터링, 애플리케이션 제어 등의 기능을 수행할 수 있습니다. <br><br>
요약하면, WAF는 주로 웹 애플리케이션 레벨에서 보안을 강화하는 데 사용되고,  <br>
Proxy 구조는 네트워크 레벨에서 다양한 목적으로 중계 및 보안 기능을 수행하는 데 사용됩니다. <br>
Proxy 서버는 WAF와 함께 사용되어 웹 보안을 더욱 강화할 수 있습니다.
- Proxy 서버는 클라이언트와 서버 사이에서 중계 역할을 하는 서버 또는 소프트웨어입니다.
- 네트워크 레벨에서 동작하며, 클라이언트 요청을 대신 받아서 서버로 전달하거나 서버 응답을 대신 받아서 클라이언트로 전달합니다.
- Proxy는 네트워크 트래픽을 중개하므로 주로 네트워크 레벨의 기능을 제공합니다.
- Proxy 구조는 웹 캐싱, 로드 밸런싱, 익명화, 보안 검사, 액세스 제어와 같은 다양한 용도로 사용될 수 있습니다.

## SSL인증서는 어디에 설치되나?
SSL 인증서는 웹 서버에서 설치됩니다.  <br>
구체적으로 말하면, SSL 인증서는 웹 서버 소프트웨어 (예: Apache, Nginx, Microsoft IIS 등)의 설정에서 설정됩니다. <br><br>
일반적으로 SSL 인증서를 설치하는 단계는 다음과 같습니다.
### 1) SSL 인증서 획득
- 먼저 SSL 인증서를 인증 기관 (Certificate Authority, CA) 또는 자체 서명 CA로부터 구입하거나 발급받아야 합니다. 
- 인증서는 공개 키 및 개인 키로 구성되며, 이러한 키 쌍은 서버에서 생성하고 인증 기관에 의해 인증됩니다.
### 2) 인증서 파일 생성
- 서버에서 SSL 인증서를 사용하려면 개인 키 및 인증서 파일을 생성해야 합니다. 
- 이 파일은 웹 서버 소프트웨어에서 사용됩니다.
### 3) 웹 서버 구성
- 웹 서버 소프트웨어 (예: Apache 또는 Nginx)의 설정 파일을 열어 SSL 인증서 파일의 경로와 서버에서 사용할 포트 (일반적으로 443)를 지정합니다. 
- 또한 인증서와 개인 키 파일을 함께 설정 파일에 추가해야 합니다.
### 4) 웹 서버 재시작
SSL 인증서를 설치한 후에는 웹 서버를 다시 시작하여 변경 사항을 적용합니다.

### 5) 테스트 및 모니터링
- SSL 인증서를 제대로 설치했는지 확인하기 위해 웹 브라우저를 사용하여 웹 사이트에 접속하고 SSL 연결을 확인합니다. 
- 또한 SSL 인증서의 만료일을 모니터링하여 인증서 갱신을 계획합니다.

<br>

SSL 인증서는 웹 서버의 보안 통신을 활성화하고, 클라이언트와 서버 간의 데이터 전송을 암호화하며,  <br>
웹 사이트 방문자에게 안전한 연결을 제공하는 데 중요한 역할을 합니다. <br>
따라서 올바르게 설치하고 관리하는 것이 중요합니다.<br>

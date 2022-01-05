# HTTP 웹 기본 지식

`실무에 꼭 필요한 HTTP의 기본 지식 및 핵심 내용을 습득할 수 있도록 정리한다.`

**[ 해당 강의를 수강한 이유 ]**

모든 것이 HTTP 기반 위에서 동작한다. HTML, image, 영상, 파일뿐만 아니라 앱과 서버가 통신할 때, 서버와 서버가 통신할 때도 http 프로토콜 위에서 데이터를 주고받는다. 특히 백엔드 개발자 같은 경우 웹 기술들이 모두 http를 기반으로 구현되어 있기 때문에 http를 제대로 이해하지 못한 상태에서 웹 기술에 대한 공부를 하면 깊이 있게 원리를 파악하기 쉽지 않다. 

## 1. 인터넷 네트워크

### 1.1. IP
- 인터넷 프로토콜 역할
- 지정한 IP 주소(IP Address)에 데이터 전달
- 패킷(Packet)이라는 통신 단위로 데이터 전달

#### [ IP 프로토콜의 한계 ]
- **비연결성**
  - 패킷을 받을 대상이 없거나 서비스가 불능 상태여도 패킷 전송
    - 대상 서버가 패킷을 받을 수 있는 상태인지 모름
- **비신뢰성**
  - 전송 과정에서 패킷이 소실될 수 있다.
  - 패킷 전달 순서 문제 발생
- **프로그램 구분**
  - 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?

### 1.2. TCP, UDP

**[ 인터넷 프로토콜 스택의 4계층 ]**

| 4계층 |
| ------------------------ |
| 애플리케이션 계층 - HTTP, FTP |
| 전송 계층 - TCP, UDP |
| 인터넷 계층 - IP |
| 네트워크 인터페이스 계층 |

**[ TCP 특징 ]**

**: 전송 제어 프로토콜 (Transmission Control Protocol)**
- 연결지향 - TCP 3 way handshake (가상 연결)
  - 클라이언트가 접속 요청을(SYN) 하고 -> 서버에서 응답과 요청을 한다.(SYN + ACK) -> 클라이언트는 요청에 응답(ACK)한다.
  - 1.SYN -> 2.SYN+ACK -> 3.ACK 의 과정을 통해 대상 서버가 통신할 수 있는 상태인지 확인 할 수 있다.
  - `참고 - 3.ACK와 함께 데이터 전송 가능`
- 데이터 전달 보증
  - 클라이언트에서 데이터를 전송하면, 서버에서 요청의 결과를 응답해준다.
- 순서 보장
- 신뢰할 수 있는 프로토콜
- 현재는 대부분 TCP 사용

**[ UDP 특징 ]**

**: 사용자 데이터그램 프로토콜 (User Datagram Protocol)**
- 기능이 거의 없다. (하얀 도화지에 비유)
- 연결지향 - TCP 3 way handshake X
- 데이터 전달 보증 X
- 순서 보장 X
- 단순하고 빠름
- 정리
  - IP와 거의 같다. (+ ORT 정도만 추가)
  - 애플리케이션 추가 작업 필요

**[ 패킷 정보 ]**
- IP 패킷 정보
  - IP 패킷 -> 출발지 IP, 목적지 IP, 기타..
  - 전송데이터
- TCP/IP 패킷 정보
  - IP 패킷 -> 출발지 IP, 목적지 IP, 기타..
  - TCP 세그먼트 -> 출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보 등..
  - 전송데이터

### 1.3. PORT
**: 같은 IP 내에서 프로세스 구분 (ex - ip : 아파트 / port : 동,홋수)**
- 0 ~ 65535 할당 가능
- 0 ~ 1023: 잘 알려진 포트, 사용하지 않는 것이 좋음
  - http: 80
  - https: 443
  
### 1.4. DNS
**: 도메인 네임 시스템(Domain Name System)**

- IP 사용의 단점을 해결 (도메인명을 IP 주소로 변환)
    - IP는 기억하기 어렵다.
    - IP는 변경될 수 있다.
    

## **2. URI와 웹 브라우저 요청 흐름**

### 2.1. URI

**[ URI ]**

- **URI : Uniform Resource Identifier**
- **U**niform: 리소스를 식별하는 통일된 방식
- **R**esource: 자원, URI로 식별할 수 있는 모든 것 (제한 없음)
- **I**dentifier: 다른 항목과 구분하는데 필요한 정보
- URI는 로케이터(locator), 이름(name) 또는 둘 다 추가로 분류될 수 있다.

**[ URL, URN ]**
- **URL : Uniform Resource Locator**
- **URN : Uniform Resource Name**
- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않는다.
- urn:isbn:8960777331 (특정 책의 isbn URN)
- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

**[URL 분석]**

- scheme://[userinfo@]host[:port][/path][?query][#fragment]
- https://www.google.com:443/search?q=hello&hl=ko

<br> 

- **scheme**
  - 주로 프로토콜 사용
  - 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
    - 종류 - http, https, ftp 등등
  - https는 http에 보안 추가 (HTTP Secure)
- **userinfo**
  - URL에 사용자 정보를 포함해서 인증
  - 거의 사용하지 않음
- **host**
  - 호스트명
  - 도메인명 또는 IP 주소를 직접 사용가능
- **PORT**
  - 포트(PORT)
  - 접속 포트
  - 일반적으로 생략 (생략시 http는 80, https는 443)
- **path**
  - 리소스 경로(path)
  - 계층적 구조
- **query**
  - key=value 형태
  - ?로 시작, &로 추가 가능 ?keyA=valueA&keyB=valueB
  - query parameter, query string 등으로 불림
- **fragment**
  - https://docs.spring.io/spring-boot/docs/current/reference/html/getting-
    started.html#getting-started-introducing-spring-boot
  - html 내부 북마크 등에 사용
  - 서버에 전송하는 정보 아님
  
### 2.2. 웹 브라우저 요청 흐름

**[ HTTP 메시지 전송 ]**

1. 웹 브라우저가 HTTP 메시지 생성
2. SOCKET 라이브러리를 통해 전달
   - A: TCP/IP 연결 (IP, PORT)
   - B: 데이터 전달
3. TCP/IP 패킷 생성, HTTP 메시지 포함
4. 웹 브라우저 -> (요청 패킷 전달) -> 서버
5. 웹 브라우저 -> (요청 패킷 도착) -> 서버
6. 웹 브라우저 <- (응답 패킷 전달) <- 서버
7. 웹 브라우저 <- (응답 패킷 도착) <- 서버
8. 웹 브라우저 HTML 렌더링

## **3. HTTP 기본**

**[ HTTP ]**

: HyperText Transfer Protocol

- **HTTP/1.1 : 가장 많이 사용, 가장 중요한 버전**
  - RFC2068 (1997년) -> RFC2616 (1999년) -> RFC7230 ~ 7235 (2014년)
  - HTTP/2 : 성능 개선 (2015년)
  - HTTP/3 : TCP 대신 UDP 사용, 성능 개선 (진행중)
- **기반 프로토콜**
  - **TCP**: HTTP/1.1, HTTP/2
  - **UDP**: HTTP/3

**[ HTTP 특징 ]**
- 클라이언트 서버 구조
- 무상태 프로토콜(stateless), 비연결성(connectionless)
- HTTP 메시지
- 단순함, 확장 가능

### 3.1. 클라이언트 서버 구조

- Request / Response 구조
- 클라이언트는 서버에 요청을 보내고, 응답을 대기
- 서버가 요청에 대한 결과를 만들어서 응답

### 3.2. Stateful, Stateless

**[ Stateless ]**
- 무상태 프로토콜
- 서버가 클라이언트의 상태를 보존 X
- 장점: 서버 확장성 높음 (스케일 아웃)
- 단점: 클라이언트가 추가 데이터 전송

**[ Stateful, Stateless 차이 ]**
- **상태 유지 (Stateful)**: 항상 같은 서버가 유지되어야 한다.
- **무상태 (Stateless)**: 아무 서버나 호출해도 된다.
  - 갑자기 고객이 증가해도, 클라이언트 요청이 증가해도 서버를 대거 투입할 수 있다. -> 스케일 아웃 (수평 확장 유리)
- 무상태는 응답 서버를 쉽게 바꿀 수 있다.
  - **무한한 서버 증설 가능**

**[ Stateless 한계 ]**
- 무상태로 설계할 수 없는 경우도 있다.
- 무상태
  - 예) 로그인이 필요 없는 단순한 서비스 소개 화면
- 상태 유지
  - 예) 로그인
- 로그인한 사용자의 경우 로그인 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용

### 3.3. 비연결성 (connectionless)
- HTTP는 기본이 연결을 유지하지 않는 모델
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
  - 예) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
- 서버 자원을 매우 효율적으로 사용할 수 있음

**[ 한계와 극복 ]**
- TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등 수 많은 자원이 함께 다운로드
- 지금은 HTTP 지속 연결(Persistent Connections)로 문제 해결
- HTTP/2, HTTP/3에서 더 많은 최적화

### 3.4. HTTP 메시지

- **거의 모든 형태의 데이터 전송 가능**
  - HTML, TEXT
  - IMAGE, 음성, 영상, 파일
  - JSON, XML (API)
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP 사용

**[ 시작 라인 ]**
- **start-line = request-line / status-line**
- **요청 메시지**
  - GET /search?q=hello&hl=ko HTTP/1.1
    - **request-line** = method SP(공백) request-target SP HTTP-version CRLF(엔터)
      - HTTP 메서드 (GET)
      - 요청 대상 (/search?q=hello&hl=ko)
      - HTTP version (HTTP/1.1)
- **응답 메시지**
  - HTTP/1.1 200 OK
    - **status-line** = HTTP-version SP status-code SP reason-phrase CRLF
      - HTTP 버전 (HTTP/1.1)
      - HTTP 상태 코드 (200)
      - 이유 문구 (OK)

**[ HTTP 헤더 ]**
- header-field = field-name ":" OWS field-value OWS (OWS: 띄어쓰기 허용)
- field-name은 대소문자 구분 없음
  - Host: www.google.com
  - Content-Type: text/html;charset=UTF-8
  - Content-Length: 3423
- **용도**
  - HTTP 전송에 필요한 모든 부가정보
    - 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 애플리케이션 정보, 캐시 관리 정보 등
  - 표준 헤더가 너무 많음
    - `참고 - https://en.wikipedia.org/wiki/List_of_HTTP_header_fields`
  - 필요시 임의의 헤더 추가 가능
    - 예) strawberry-cookie: yummy

**[ HTTP 메시지 바디 ]**
- **용도**
  - 실제 전송할 데이터
  - HTML 문서, 이미지, 영상, JSON 등 byte로 표현할 수 있는 모든 데이터 전송 가능
  

## **4. HTTP 메서드**


## **5. HTTP 메서드 활용**


## **6. HTTP 상태코드**


## **7. HTTP 헤더1 - 일반 헤더**


## **8. HTTP 헤더2 - 캐시와 조건부 요청**


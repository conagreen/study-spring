# HTTP 웹 기본 지식

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

### 4.1. HTTP API

**[ API URI 설계 ]**

**이것은 좋은 URI 설계인가?**

1. 회원 목록 조회 /read-member-list
2. 회원 조회 /read-member-by-id
3. 회원 등록 /create-member
4. 회원 수정 /update-member
5. 회원 삭제 /delete-member

( URI를 설계할 때 가장 중요한 것은 `리소스 식별`하는 것 )

- 리소스의 의미
  - 회원을 등록하고 수정하고 조회하는 행위는 리소스가 아니다.
  - 예) 회원을 조회 -> **회원이라는 개념 자체가 바로 리소스**
- 리소스와 행위 분리
  - **URI는 회원이라는 리소스만 식별**
  - **리소스**와 해당 리소스를 대상으로 하는 **행위**를 분리
    - 리소스: 회원
    - 행위(메서드): 조회, 등록, 삭제, 변경
  

**리소스 중심 API URI 설계**

1. **회원** 목록 조회 /members - GET
2. **회원** 조회 /members/{id} - GET
3. **회원** 등록 /members/{id} - POST
4. **회원** 수정 /members/{id} - PUT / PATCH
5. **회원** 삭제 /members/{id} - DELETE

- 리소스 식별, URI 계층 구조 활용
- 계층 구조상 상위를 컬렉션으로 보고 복수단어 사용 권당 (member->members)

**[ HTTP 메서드 종류 ]**
- **주요 메서드**
  - GET - 리소스 조회
  - POST - 요청 데이터 처리, 주로 등록에 사용
  - PUT - 리소스를 대체, 해당 리소스가 없으면 생성 (완전히 데이터 전체를 덮어버림)
  - PATCH - 리소스 부분 변경
  - DELETE - 리소스 삭제
- 기타 메서드
  - HEAD - GET과 동일하지만 메시지 부분을 제외하고, 상태 라인과 헤더만 반환
  - OPTIONS - 대상 리소스에 대한 통신 가능 옵션(메서들)을 설명 (주로 CORS에서 사용)
  - CONNECT - 대상 자원으로 식별되는 서버에 대한 터널을 설정
  - TRACE - 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

### 4.2. HTTP 메서드 - GET, POST

**[ GET ]** <br>
- 리소스 조회
- 서버에 전달하고 싶은 데이터는 query(뭐리 파라미터, 쿼리 스트링)를 통해서 전달
- 메시지 바디를 사용해서 데이터를 전달할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않는다.

요청 예시 
```
GET /members/100 HTTP/1.1 
Host: localhost:8080
```

응답 예시
```
HTTP/1.1 200 OK 
Content-Type: application/json 
Content-Length: 34

{
"username": "young", 
"age": 20
}
```

<br>


**[ POST ]**
- **메시지 바디를 통해 서버로 요청 데이터 전달**
- 서버는 요청 데이터를 처리
  - 메시지 바디를 통해 들어온 데이터를 처리하는 모든 기능을 수행한다.
- 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
  1. **새 리소스 생성 (등록)**
     1. 서버가 아직 식별하지 않은 새 리소스 생성
  2. **요청 데이터 처리**
     1. 단순히 데이터를 생성하거나, 변경하는 것을 넘어 프로세스를 처리해야 하는 경우
     2. 예) 준문에서 결제완료 -> 배달시작 -> 배달완료 처럼 단순히 값 변경을 넘어 프로세스의 상태가 변경되는 경우
     3. POST의 결과로 새로운 리소스가 생성되지 않을 수도 있음
     4. 예) POST /orders/{orderId}/start-delivery **(컨트롤 URI)**
  3. **다른 메서드로 처리하기 애매한 경우**
     1. 예) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
     2. 애매하면 POST
     
요청 예시
```
POST /members HTTP/1.1
Content-Type: application/json

{
"username": "young",
"age": 20
}
```

응답 예시
```
HTTP/1.1 201 Created 
Content-Type: application/json 
Content-Length: 34
Location: /members/100

{
"username": "young", 
"age": 20
}
```

### 4.3. HTTP 메서드 - PUT, PATCH, DELETE

**[ PUT ]**
- **리소스를 대체**
  - 리소스가 있으면 대체
  - 리소스가 없으면 생성
  - 쉽게 이야기해서 덮어버림
- **중요! 클라이언트가 리소스를 식별**
  - 클라이언트가 리소스 위치를 알고 URI 지정
  - POST와 차이점

**[ PATCH ]**
- 리소스 부분 변경
- PUT과 PATCH의 차이
  - PATCH: '부분' 변경
  - PUT: '완전히' 대체 (덮어쓰기 같은?)
  
> 개인 프로젝트를 진행하면서 회원 정보 수정, 게시글 수정처럼 데이터의 부분 변경이 필요한 경우 매번 PUT 메서드만 이용했었는데, PATCH 메서드도 적절히 활용을 해야겠다. PATCH 메서드 같은 경우 PUT 이후에 나왔다고 하니, 적지 않은 분들이 "수정은 PUT 이지!" 라고 생각할 수 있겠다.

**[ DELETE ]**
- 리소스 제거

### 4.4. HTTP 메서드의 속성

- **안전 (Safe Methods)**
  - 호출해도 리소스를 변경하지 않는다.
- **멱등 (Idempotent Methods)**
  - f(f(x)) = f(x)
  - 한 번 호출하든 두 번 호출하든 100번 호출하든 결과가 동일
  - 멱등 메서드
    - **GET**: 한 번 조회하든, 두 번 조회하든 같은 결과가 조회된다.
    - **PUT**: 결과를 대체한다. 따라서 같은 요청을 여러번 해도 최종 결과는 같다.
    - **DELETE**: 결과를 삭제한다. 같은 요청을 여러번 해도 삭제된 결과는 똑같다.
    - **POST**: 멱등이 X, 두 번 호출하면 같은 결제가 중복해서 발생할 수 있다.
- **캐시가능 (Cacheable Methods)**
  - GET, HEAD, POST, PATCH 캐시가능
  - 실제로는 GET, HEAD 정도만 캐시로 사용
    - POST, PATCH는 본문 내용까지 캐시 키로 고려해야 하는데, 구현이 쉽지 않음
  

## **5. HTTP 메서드 활용**
### 5.1. 클라이언트에서 서버로 데이터 전송

**[ 2가지 방식 ]**
- **쿼리 파라미터를 통한 데이터 전송**
  - GET
  - 주로 정렬 필터 (검색어)
- **메시지 바디를 통한 데이터 전송**
  - POST, PUT, PATCH
  - 회원 가입, 상품 주문, 리소스 등록, 리소스 변경
  
**[ 4가지 상황 ]**
- **정적 데이터 조회**
  - 이미지, 정적 텍스트 문서
  - 정적 데이터는 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회 가능
- **동적 데이터 조회**
  - 주로 검색, 게시판 목록에서 정렬 필터 (검색어)
  - 조회 조건을 줄여주는 필터, 조회 결과를 정렬하는 조건에 주로 사용
  - GET은 쿼리 파라미터 사용해서 데이터를 전달
- **HTML Form을 통한 데이터 전송**
  - POST 전송
    - 예) 회원 가입, 상품 주문, 데이터 변경
    - Content-Type: application/x-www-form-urlencoded 사용
      - form의 내용을 메시지 바디를 통해 전송 (key=value, 쿼리 파라미터 형식)
      - 전송 데이터를 url encoding 처리
  - GET 전송
    - Content-Type: multipart/form-data
      - 파일 업로드 같은 바이너리 데이터 전송시 사용
      - 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능
  - 참고: HTML Form 전송은 **GET, POST만 지원**
- **HTTP API를 통한 데이터 전송**
  - 서버 to 서버 (백엔드 시스템 통신)
  - 앱 클라이언트 (아이폰, 안드로이드)
  - 웹 클라이언트
    - HTML에서 FORM 전송 대신 자바 스크립트를 통한 통신에 사용(AJAX)
    - 예) React, VueJs 같은 웹 클라이언트와 API 통신
  - POST, PUT, PATCH: 메시지 바디를 통해 데이터 전송
  - GET: 조회, 쿼리 파라미터로 데이터 전달
  - Content-Type: application/json을 주로 사용 (사실상 표준)
    - TEXT, XML, JSON 등등


### 5.2. HTTP API 설계 예시

**[ HTTP API - 컬렉션 ]**
- **POST 기반 등록**
- 클라이언트는 등록될 리소스의 URI를 모른다.
  - 회원 등록 /members -> POST
- 서버가 새로 등록된 리소스 URI를 생성해준다.
  - HTTP/1.1 201 Created <br>
    Location: **/members/100**
- 컬렉션 (Collection)
  - 서버가 관리하는 리소스 디렉토리
  - 서버가 리소스의 URI를 생성하고 관리
  - 여기서 컬렉션은 /members


**[ HTTP API - 스토어 ]**
- **PUT 기반 등록**
- 클라이언트가 리소스 URI를 알고 있어야 한다.
  - 파일 등록 /files/star.jpg -> PUT
- 클라이언트가 직접 리소스의 URI를 지정한다.
- 스토어 (Store)
  - 클라이언트가 관리하는 리소스 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
  - 여기서 스토어는 /files
  
## **6. HTTP 상태코드**

**[ 상태 코드 ]** <br>
**: 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능**
- 1xx (Informational): 요청이 수신되어 처리중 (거의 사용하지 x)
- 2xx (Successful): 요청 정상 처리
- 3xx (Redirection): 요청을 완료하려면 추가 행동이 필요
- 4xx (Client Error): 클라이언트 오류, 잘못된 문법 등으로 서버가 요청을 수행할 수 없음
- 5xx (Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함
- 
### 6.1. 2xx - 성공
**[ 2xx - Successful ]** <br>
**: 클라이언트의 요청을 성공적으로 처리**
- **200 OK** : 요청 성공
- **201 Created** : 요청 성공해서 새로운 리소스가 생성됨
- **202 Accepted** : 요청이 접수되었으나 처리가 완료되지 않았음
  - 배치 처리 같은 곳에서 사용
  - 예) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함
- **204 No content** : 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 본문에 보낼 데이터가 없음
  - 예) 웹 문서 편집기에서 save 버튼
  - save 버튼의 결과로 아무 내용이 없어도 된다.
  - save 버튼을 눌러도 같은 화면을 유지해야 한다.
  - 결과 내용이 없어도 204 메시지(2xx)만으로 성공을 인식할 수 있다.

### 6.2. 3xx - 리다이렉션
**[ 3xx - Redirection ]** <br>
**: 요청을 완료하기 위해 유저 에이전트의 추가 조치 필요**
- 300 Multiple Choices
- 301 Moved Permanently
- 302 Found
- 303 See Other
- 304 Not Modified
- 307 Temporary Redirect
- 308 Permanent Redirect

**< 리다이렉션 >**
- 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동 (리다이렉트)
- **종류**
  - **영구 리다이렉션** - 특정 리소스의 URI가 영구적으로 이동
    - 예) /members -> /users
    - 예) /event -> /new-event
  - **일시 리다이렉션** - 일시적인 변경
    - 주문 완료 후 주문 내역 화면으로 이동
    - PRG: Post/Redirect/Get
  - **특수 리다이렉션**
    - 결과 대신 캐시를 사용

<br>

**< 영구 리다이렉션 - 301, 308 >**
- 리소스의 URI가 영구적으로 이동
- 원래의 URL 사용 X, 검색 엔진 등에서도 변경 인지
- **301 Moved Permanently**
  - **리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음 (MAY)**
- **308 Permanent Redirect**
  - 301과 기능은 같음
  - **리다이렉트시 요청 메서드와 본문 유지 (처음 POST를 보내면 리다이렉트도 POST 유지)**

`308은 실무에서 거의 사용하지 않는다고 함. 내부적으로 전달해야 하는 데이터 자체가 다 바뀌어버리기 때문에 웬만하면 get으로 돌린다.`


**< 일시적인 리다이렉션 - 302, 307, 303 >**
- 리소스의 URI가 일시적으로 변경
- 따라서 검색 엔진 등에서 URL을 변경하면 안됨
- **302 found**
  - **리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음 (MAY)**
- **307 Temporary Redirect**
  - 302와 기능은 같음
  - **리다이렉트시 요청 메서드와 본문 유지 (MUST NOT)**
- **303 See Other**
  - 302와 기능은 같음
  - **리다이렉트시 요청 메서드가 GET으로 변경**
- **PRG: Post/Redirect/Get**
  - POST로 주문후에 웹 브라우저를 새로고침한다면?
  - 새로고침은 이전의 요청을 재요청 하는 것 -> 따라서, 중복 주문이 될 수 있음 
  - PRG 이후 리다이렉트
    - URL이 이미 POST -> GET으로 리다이렉트 됨
    - 새로고침을 해도 GET으로 결과 화면만 조회
    - 결과: 새로고침으로 인한 중복 주문 방지

`302 Found 내용 중 'GET으로 변할 수 있음' 이라는 말이 모호하다. 변하면 변하는거고 안변하면 안변하는거지. 변할 수 있다는 건 뭘까? 역사를 살펴보면 처음 302 스펙의 의도는 HTTP 메서드를 유지하는 것 이었다. 그런데 웹 브라우저들이 대부분 GET으로 바꾸어버림..(따라서 일부는 다르게 동작할 수 있음..) 그래서 모호한 302를 대신하는 명확한 303, 307이 등장했다고 한다. (301 대응으로 308도 등장) 307, 303을 권장하지만 현실적으로 이미 많은 애플리케이션 라이브러리들이 302를 기본값으로 사용하고 있으며, 자동 리다이렉션시에 GET으로 변해도 되면 그냥 302를 사용해도 큰 문제 없다.`

**< 기타 리다이렉션 - 300, 304 >**
- 300 Multiple Choices: 안쓴단다..
- 304 Not Modified
  - 캐시를 목적으로 사용
  - 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트는 로컬 PC에 저장된 캐시를 재사용한다. (캐시로 리다이렉트)
  - 304 응답은 응답에 메시지 바디를 포함하면 X (로컬캐시를 사용해야 하므로)
  - 조건부 GET, HEAD 요청시 사용
  
### 6.3. 4xx: 클라이언트 오류, 5xx: 서버 오류
**[ 4xx - Client Error ]** <br>
**: 클라이언트 오류**
- **오류의 원인이 클라이언트에 있음**
- 중요! 클라이언트가 이미 잘못된 요청, 데이터를 보내고 있기 때문에 똑같은 재시도가 실패함
- **400 Bad Request**: 클라이언트가 잘못된 요청을 해서 서버가 요청을 처리할 수 없음
  - 요청 구문, 메시지 등등 오류
  - 클라이언트는 요청 내용을 다시 검토하고 보내야함
- **401 Unauthorized**: 클라이언트가 해당 리소스에 대한 인증이 필요함
  - 인증(Authentication) 되지 않음 (예 - 로그인)
  - 401 오류 발생시 응답에 WWW-Authenticate 헤더와 함께 인증 방법을 설명
- **403 Forbidden**: 서버가 요청을 이해했지만 승인을 거부함
  - 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
  - 예) 어드민 등급이 아닌 사용자가 로그인은 했지만, 어드민 등급의 리소스에 접근하는 경우
- **404 Not Found**: 요청 리소스를 찾을 수 없음
  - 요청 리소스가 서버에 없음
  - 또는 클라이언트가 권한이 부족한 리소스에 접근했을 경우 해당 리소스를 숨기고 싶을 때
  
**[ 5xx - Server Error ]** <br>
**: 서버 오류**
- 서버에 문제가 있기 때문에 재시도하면 성공할 수도 있음 (복구가 되거나 등)
- **500 Internal Server Error**: 서버 문제로 오류 발생, 애매하면 500 오류 
- **503 Service Unavailable**: 서비스 이용 불가
  - 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음
  - Retry-After 헤더 필드로 얼마뒤에 복구되는지 보낼 수 있음


## **7. HTTP 헤더1 - 일반 헤더**

**[ RFC723x 변화 ]**
- 엔티티(Entity) -> 표현(Representation)
- 표현 = 표현 메타데이터 + 표현 데이터

**[ HTTP BODY ]**
- 메시지 본문(message body)을 통해 표현 데이터 전달
- 메시지 본문 = 페이로드 (payload)
- **표현**은 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더는 표현 데이터**를 해석할 수 있는 정보 제공
  - 데이터 유형(html, json), 데이터 길이, 압축 정보 등등
  
### 7.1. 표현
- **Content-Type: 표현 데이터의 형식**
  - 미디어 타입, 문자 인코딩
  - text/html; charset=utf-8
  - application/json
  - image/png
- **Content-Encoding: 표현 데이터의 압축 방식**
  - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더 추가
  - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축 해제
  - gzip
  - deflate
  - identity
- **Content-Language: 표현 데이터의 자연 언어**
  - ko
  - en
  - en-US
- **Content-Length: 표현 데이터의 길이**
  - 바이트 단위
  - Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨
    
`표현 헤더는 전송, 응답 둘 다 사용`

### 7.2. 콘텐츠 협상

**[ 콘텐츠 네고시에이션 ]**<br>
**: 클라이언트가 선호하는 표현 요청**
- Accept: 클라이언트가 선호하는 미디어 타입 전달
- Accept-Charset: 클라이언트가 선호하는 문자 인코딩
- Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
- Accept-Language: 클라이언트가 선호하는 자연 언어

`협상 헤더는 요청시에만 사용`

<br>

**[ 협상과 우선순위 ]**<br>
- Quality Values(q) 값 사용
- 0~1, **클수록 높은 우선순위**, 생략하면 1
  - Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
    - ko-KR;q=1 (q 생략)
    - ko;q=0.9
    - en-US;q=0.8
    - en;q=0.7
- 구체적인 것이 우선
  - Accept: text/*, text/plain, text/plain;format=flowed, \*/\*
    1. text/plain;format=flowed
    2. text/plain
    3. text/*
    4. \*/\*

### 7.3. 전송 방식
- **단순 전송**
- **압축 전송**
- **분할 전송**
- **범위 전송**

### 7.4. 일반 정보
- **From: 유저 에이전트의 이메일 정보**
  - 일반적으로 잘 사용되지 X
  - 검색 엔진 같은 곳에서 주로 사용
  - `요청에서 사용`
- **Referer: 이전 웹 페이지 주소**
  - A -> B로 이동하는 경우 B를 요청할 때 Referer: A를 포함해서 요청
  - Feferer를 사용해서 유입 경로 분석 가능
  - referer는 단어 referrer의 오타
  - `요청에서 사용`
- **User-Agent: 유저 에이전트 애플리케이션 정보**
  - user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/ 537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
  - 클라이언트의 애플리케이션 정보 (웹 브라우저 정보 등등)
  - 통계 정보
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
  - `요청에서 사용`
- **Server: 요청을 처리하는 ORIGIN 서버의 소프트웨어 정보**
  - Server: Apache/2.2.22 (Debian)
  - server: nginx
  - `응답에서 사용`
- **Date: 메시지가 발생한 날짜와 시간**
  - Date: Tue, 15 Nov 1994 08:12:31 GMT
  - `응답에서 사용`

### 7.5. 특별한 정보 
- **Host: 요청한 호스트 정보 (도메인)**
  - 필수
  - 하나의 서버가 여러 도메인을 처리해야 할 때
  - 하나의 IP 주소에 여러 도메인이 적용되어 있을 때
  - `요청에서 사용`
- **Location: 페이지 리다이렉션**
  - 201 (Created): Location 값은 요청에 의해 생성된 리소스 URI
  - 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴
- **Allow: 허용 가능한 HTTP 메서드**
  - 405 (Method Not Allowed)에서 응답에 포함해야함
  - Allow: GET, HEAD, PUT
- **Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간**
  - 503 (Service Unavailable): 서버스가 언제까지 불능인지 알려줄 수 있음
  - Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
  - Retry-After: 120 (초단위 표기)

### 7.6. 쿠키

- Set-Cookie: 서버에서 클라이언트로 쿠키 전달 (응답)
- Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달
- 예) set-cookie: **sessionId**=abcde1234; **expires**=Sat, 26-Dec-2020 00:00:00 GMT; **path**=/; **domain**=.google.com; **Secure**
- 사용처
  - 사용자 로그인 세션 관리
  - 광고 정보 트래킹
- 쿠키 정보는 항상 서버에 전송됨
  - 네트워크 트래픽 추가 유발
  - 최소한의 정보만 사용 (세션 id, 인증 토큰)
  - 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고
- 주의! 보안에 민감한 데이터는 저장하면 X (주민번호, 신용카드 번호 등)

**[ 생명주기 ]**
- **Expires**
  - Set-Cookie: **expires**=Sat, 26-Dec-2020 04:39:21 GMT
  - 만료일이 되면 쿠키 삭제
- **max-Age**
  - Set-Cookie: **max-age**=3600 (3600초)
  - 0이나 음수를 지정하면 쿠키 삭제
- 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
- 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

**[ 도메인 ]**
- 예) domain=example.org
- **명시: 명시한 문서 기준 도메인 + 서브 도메인 포함**
  - domain=example.org를 지정해서 쿠키 생성
    - domain=example.org는 물론이고, dev.example.org도 쿠키 접근
- **생략: 현재 문서 기준 도메인만 적용**
  - example.org에서 쿠키를 생성하고 domain 지정을 생략
    - example.org에서만 쿠키 접근, dev.example.org는 쿠키 미접근

**[ 경로 ]**
- 예) path=/home
- **이 경로를 포함한 하위 경로 페이지만 쿠키 접근**
- **일반적으로 path=/ 루트로 지정**

**[ 보안 ]**
- Secure
  - Secure를 적용하면 https인 경우에만 전송
- HttpOnly
  - XSS 공격 방지
  - 자바스크립트에서 접근 불가 (document.cookie)
  - HTTP 전송에만 사용
- SameSite
  - XSRF 공격 방지
  - 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송

## **8. HTTP 헤더2 - 캐시와 조건부 요청**

### 8.1. 캐시 기본 동작
**[ 캐시가 없을 때 ]**
- 데이터가 변경되지 않이도 네트워크를 통해 데이터를 다운로드 받는다.
- 인터넷 네트워크는 매우 느리고 비싸다.
- 브라우저 로딩 속도가 느리다. -> 느린 사용자 경험

**[ 캐시 적용 ]**
- 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
- 비싼 네트워크 사용량을 줄일 수 있다.
- 브라우저 로딩 속도가 매우 빠르다. -> 빠른 사용자 경험

**[ 캐시 시간 초과 ]**
- 캐시 유효 시간이 초과하면, 서버를 통해 데이터를 다시 조회하고 캐시를 갱신한다.
- 이때 다시 네트워크 다운로드가 발생한다.

### 8.2. 검증 헤더와 조건부 요청
**[ 캐시 시간 초과 ]**
- 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타난다.
  1. 서버에서 기존 데이터를 변경 함
  2. 서버에서 기존 데이터를 변경하지 않음
     - 데이터를 전송하는 대신 저장해 두었던 캐시를 재사용할 수 있다.
     - 단, 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있어야 함.
- 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면 304 Not Modified + 헤더 메타 정보만 응답 (바디 X)
- 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
- 클라이언트는 캐시에 저장되어 있는 데이터 재활용
- 결과적으로 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드

<br>

- **검증 헤더**
  - 캐시 데이터와 서버 데이터가 같은지 검증하는 데이터
  - Last-Modified, ETag
- **조건부 요청 헤더**
  - 검증 헤더로 조건에 따른 분기
    - If-Modified-Since: Last-Modified / If-None-Match: ETag 사용사용
      - 조건이 만족하면 200 OK
      - 조건이 만족하지 않으면 304 Not Modified

**[ Last-Modified, If-Modified-Since 단점 ]**
- 1초 미만 단위로 캐시 조정이 불가능 (큰 단점은 x)
- 날짜 기반의 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  - 예) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우

**[ ETag, If-None-Match ]**
- ETag(Entity Tag)
- 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
  - 예) ETag: "v1.0", ETag: "a2jiodwjekjl3"
- 데이터가 변경되면 이 이름을 바꾸어서 변경함 (Hash를 다시 생성)
  - 예) ETag: "aaaaa" -> ETag: "bbbbb"
- 단순하게 ETag만 보내서 같으면 유지, 다르면 다운로드!
- **캐시 제어 로직을 서버에서 완전히 관리**
- 클라이언트는 단순히 이 값을 서버에 제공 (클라이언트는 캐시 메커니즘을 모름)
  - 예) 서버는 배타 오픈 기간인 3일 동안 파일이 변경되어도 ETag를 동일하게 유지
  - 애플리케이션 배포 주기에 맞추어 ETag 모두 갱신

### 8.3. 캐시와 조건부 요청 헤더
**[캐시 제어 헤더]**
- **Cache-Control: 캐시 제어**
  - Cache-Control: max-age
    - 캐시 유효 시간, 초 단위
  - Cache-Control: no-cache
    - 데이터는 캐시해도 되지만, 항상 원(origin) 서버에 검증하고 사용
  - Cache-Control: no-store
    - 데이터에 민감한 정보가 있으므로 저장하면 안됨 (메모리에서 사용하고 최대한 빨리 삭제)
- **Pragma: 캐시 제어 (하위 호환)**
  - Pragma: no-cache
  - HTTP 1.0 하위 호환
- **Expires: 캐시 유효 기간 (하위 호환)**
  - **캐시 만료일 지정**
  - expires: Mon, 01 Jan 1990 00:00:00 GMT
  - 캐시 만료일을 정확한 날짜로 지정
  - HTTP 1.0 부터 사용
  - 지금은 더 유연한 Cache-Control: max-age 권장
  - Cache-Control: max-age와 함께 사용한다면 Expires는 무시
  
### 8.4. 프록시 캐시
- **Cache-Control: public**
  - 응답이 public 캐시에 저장되어도 됨
- **Cache-Control: private**
  - 응답이 해당 사용자만을 위한 것, private 캐시에 저장해야 함 (기본 값)
- **Cache-Control: s-maxage**
  - 프록시 캐시에만 적용되는 max-age
- **Age: 60 (HTTP 헤더)**
  - 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간 (초)
  
### 8.5. 캐시 무효화

**[ 확실한 캐시 무효화 응답 ]**
- **Cache-Control: no-cache, no-store, must-revalidate**
- **Pragma: no-cache**
  - HTTP 1.0 하위 호환

- **Cache-Control: must-revalidate**
  - 캐시 만료후 최초 조회시 **원 서버에 검증**해야함
  - 원 서버 접근 실패시 반드시 오류가 발생해야 함 - 504 (Gateway Timeout)
  - must-revalidate는 캐시 유효 시간이라면 캐시를 사용함

`no-cache의 경우 원 서버에 접근할 수 없는 경우 캐시 서버 설정에 따라서 캐시 데이터를 반환할 수 있으므로 (오류 보다는 오래된 데이터라도 보여주자!) 매우 중요한 결과라면 완전한 캐시 무효화를 위리 must-revalidate 필수!`

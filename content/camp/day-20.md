+++
title = "[내일배움캠프] Spring 기초 1"
date = "2025-03-18T01:09:17+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "Spring"]
+++

---

## 네트워크(Network)

### 인터넷(Internet)
전 세계의 컴퓨터들이 연결된 거대한 네트워크.

#### 인터넷의 핵심 요소:
- ISP(Internet Service Provider, 인터넷 서비스 제공자)
    - ISP를 통해 네트워크를 사용할 수 있음.
- 라우터(Router)
    - 여러 개의 네트워크를 연결해주는 장치.
    - 데이터를 목적지까지 전달하는 역할을 함.
- 서버(Server) & 클라이언트(Client)
    - 서버: 요청을 처리하고 응답을 주는 역할.
    - 클라이언트: 요청을 보내고 응답을 받는 역할.

<br>

### 인터넷 프로토콜 IP(Internet Protocol)
인터넷에서 데이터를 전송하는 규칙
- 데이터를 목적지까지 전달하기 위해 경로(Route)를 설정 함.
- 각 컴퓨터에 고유한 주소(IP Address)를 할당 함.
- 패킷(Packet)이라는 작은 단위로 데이터를 나누어 전송함.
- IP 자체만으로는 데이터의 신뢰성을 보장하지 않음. 
- 패킷이 순서대로 도착하거나 손실되지 않는다는 보장은 TCP 같은 상위 프로토콜에서 처리함.

<br>

### IP 방식의 문제점
- 비연결성
    - IP는 패킷을 목적지까지 보내지만, 제대로 도착했는지 확인하지 않음.
    - 데이터를 순서대로 보장하지 않음.
- 신뢰성 부족
    - 패킷이 손실될 수 있음.
    - 데이터가 중간에 유실될 경우 재전송이 필요하지만, IP 자체는 이를 처리하지 않음.
- 패킷 분실 가능성
    - 네트워크가 혼잡하면 패킷이 사라질 수 있음.

<br>

### TCP(Transmission Control Protocol)
서버와 클라이언트 간에 데이터를 신뢰성 있게 전달하기 위해 만들어진 프로토콜.
- 연결 지향
    - 데이터를 보내기 전에 3-way handshake(삼중 핸드셰이크) 를 사용하여 연결을 설정함.
- 신뢰성 보장
    - 데이터가 손실되지 않도록 확인 응답(ACK)을 사용하고, 손실된 패킷을 재전송함.
- 순서 보장
    - 패킷이 순서대로 도착하도록 관리함.
- 흐름 제어 및 혼잡 제어
    - 네트워크 속도 차이를 고려하여 데이터를 조절하고, 네트워크가 과부하 상태가 되지 않도록 조정함.

<br>

### UDP(User Datagram Protocol)
비연결형, 신뢰성이 없는 전송 프로토콜.
- 비연결형
    - 데이터를 보내기 전에 연결을 설정하지 않음.
- 빠른 전송
    - 속도가 빠르지만, 신뢰성이 부족함.
    - 패킷이 유실될 수 있어도 다시 전송하지 않음.
- 순서 보장 없음
    - 패킷이 순서대로 도착한다는 보장이 없음.

<br>

### PORT
같은 IP 내에서 프로세스 구분을 하기 위해서 사용.
- 자주 사용되는 PORT
    1. 0 ~ 65535 할당 가능
    2. 이미 사용되고 있는 포트 (0 ~ 1023) 
        - IANA(Internet Assigned Numbers Authority)에서 관리
        - 사용하지 않는 것이 좋음
        - `FTP` - 20, 21 (TCP)
        - `SSH` - 22 (TCP)
        - `텔넷` - 23 (TCP)
        - `SMTP` - 25 (TCP)
        - `DNS` - 53 (TCP/UDP)
        - `DHCP` - 67 (UDP)
        - `HTTP` - 80 (TCP)
        - `HTTPS` - 443 (TCP)
        - `RDP` - 3389 (TCP/UDP)

---

## 관련 용어

### JSON (데이터 교환 포맷)
텍스트 기반의 경량 데이터 형식
- 키-값(key-value) 형태
- 경량 (파일 크기가 작고 빠르게 처리 가능)
- 언어 독립적 (Java, JavaScript, Python 등 모든 언어에서 사용 가능)
- 클라이언트 to 서버의 통신에 JSON을 사용
- 서버 to 서버의 통신에도 JSON을 사용

<br>

### Scale Up, Scale Out (서버 및 네트워크 확장 관련)
서버의 성능 향상을 위한 두 가지 방법
- Scale Up (수직 확장)
    - 서버 자체의 성능을 높이는 방식
    - CPU, RAM, 디스크 용량을 늘려서 한 대의 서버가 더 많은 작업을 처리하게 함.
- Scale Out (수평 확장)
    - 여러 개의 서버를 추가하여 부하를 분산하는 방식
    - 로드 밸런서를 사용하여 여러 서버에 부하를 분산함.

<br>

### Stateful, Stateless (네트워크 세션 관리와 관련)
웹 애플리케이션이 사용자의 상태(데이터)를 어떻게 관리하는지에 대한 개념
- Stateful (상태 유지)
    - 서버가 사용자의 상태(로그인 정보, 세션 데이터 등)를 기억함.
    - 사용자가 요청할 때마다 이전 상태를 유지함.
    - 단점
      - 서버가 사용자의 상태를 계속 유지해야 해서 부하 증가
      - 여러 서버를 사용할 경우 세션 동기화 문제 발생
- Stateless (상태 없음)
    - 서버가 사용자의 상태를 저장하지 않음.
    - 사용자의 요청은 항상 독립적인 요청으로 처리됨.
    - 장점
      - 같은 서버를 유지할 필요가 없음
      - 확장성이 좋음 (Scale out 쉬움)
    - 단점
      - 매 요청마다 필요한 데이터를 보내야 함

<br>

### Connection, Connectionless (네트워크 연결 방식)
네트워크에서 데이터를 주고받을 때 연결을 유지하는지 여부에 따른 개념
- Connection (연결 지향)
    - 데이터를 주고받기 전에 연결을 먼저 설정하는 방식.
    - TCP 기반, 연결을 유지하면서 데이터를 주고받음.
    - 장점
      - 새로운 연결 과정을 거치지 않아도 된다.
      - 그만큼 요청에 대한 응답 속도가 빨라짐.
    - 단점
      - 클라이언트가 연결을 유지하지 않으면 불필요한 자원이 소모될 수 있음.
- Connectionless (비연결 지향)
    - 연결을 유지하지 않고, 데이터만 보낸 후 바로 종료하는 방식.
    - UDP 기반, 받았는지 확인하지 않음.
    - 장점
      - 서버 자원을 효율적으로 사용할 수 있음.
    - 단점
      - 추가 요청 시 새 연결(3 way handshake)로 인해 응답 시간 증가
      - 현재는 HTTP 지속 연결로 문제를 해결

<br>

### HTTP 지속연결(Persistent Connections)
- 여러 HTTP 요청을 하나의 TCP 연결에서 처리하여, 매 요청마다 새 연결을 맺는 비용을 줄임.

---

## 웹(Web) 기초

### DNS(Domain Name System)
도메인 이름을 IP 주소로 변환하는 역할.  

- 동작 과정
    - 사용자가 브라우저에 www.google.com 입력.
    - 브라우저는 DNS 서버 에 "www.google.com의 IP 주소를 알려줘!" 라고 요청.
    - DNS 서버가 142.250.190.78 같은 IP 주소를 반환.
    - 브라우저가 해당 IP 주소의 서버에 접속하여 웹 페이지를 요청.

<br>

### URI(Uniform Resource Identifier)
웹에서 특정 자원을 식별하는 고유한 주소

#### 구조:
```bash
https://www.example.com/products?id=1234
```
- 스킴(Scheme) : https:// (어떤 프로토콜을 사용할지 나타냄)
- 호스트(Host) : www.example.com (서버의 주소)
- 경로(Path) : /products (서버에서 자원의 위치)
- 쿼리(Query) : ?id=1234 (추가적인 데이터)

<br>

### URL(Uniform Resource Locator)
웹 상에서 특정 자원의 위치를 나타내는 주소

#### 구조:
```bash
https://www.example.com:8080/products?id=1234#section
```
- 프로토콜 (Protocol, Scheme) : https://
    - 데이터를 어떻게 전송할지 정하는 규칙.
    - http://, https://, ftp:// 등이 있음.
- 호스트 (Host, 도메인 이름) : www.example.com
    - 접속할 서버의 주소.
- 포트 번호 (Port, 생략 가능) : 8080
    - 서버에서 특정 서비스를 구분하는 역할.
    - 기본 포트(생략 가능): HTTP(80), HTTPS(443).
- 경로 (Path) : /products
    - 서버에서 특정 리소스가 위치한 곳.
- 쿼리 문자열 (Query String) : ?id=1234
    - 추가적인 데이터를 전달하는 부분.
- 프래그먼트 (Fragment, 앵커 태그) : #section
    - 페이지 내 특정 위치를 가리킴.

<br>

### URL과 URI의 차이점
| 구분  | URI | URL |
|-------|----|----|
| 개념 | 모든 리소스를 식별하는 주소 | 특정 리소스의 위치를 나타내는 주소 |
| 포함 관계 | URL도 URI의 일부 | URI에 속하는 개념 |
| 예시 | `mailto:user@example.com` (이메일 주소) | `https://www.example.com` (웹 주소) |

모든 URL은 URI이지만, 모든 URI가 URL은 아님

---

## HTTP & API

### HTTP(HyperText Transfer Protocol)
- 인터넷에서 데이터를 주고받는 규칙
- 다양한 데이터 유형(TEXT, IMAGE, FILE, JSON 등)을 전송할 수 있음.
- 주요 버전
    - HTTP/1.1 (가장 많이 사용됨, TCP 기반)
    - HTTP/2, HTTP/3 (성능 개선, HTTP/3는 UDP 기반)

<br>

### HTTP 특징
1. 클라이언트-서버 구조
   - 클라이언트(UI)와 서버(비즈니스 로직, 데이터 관리)를 분리하여 독립적인 발전 가능.
2. Stateless(무상태성)
   - 서버는 클라이언트의 상태를 저장하지 않음.
   - 요청마다 필요한 데이터를 포함해야 함.
   - 로그인 등 상태 유지가 필요한 경우 쿠키, 세션, 토큰 활용.
3. Connectionless(비연결성)
    - 요청 후 응답을 받으면 연결 종료.
    - 성능 최적화를 위해 Persistent Connection(지속 연결) 사용.

<br>

### HTTP Message 구조
- 요청(Request)
  ```
  GET /hello HTTP/1.1
  Host: www.example.com
  User-Agent: Mozilla/5.0
  Accept: text/html
  ```
  - Start Line: HTTP 메서드, URL, 버전  
  - Header: 요청 관련 추가 정보  
  - Body: (POST, PUT 등에서 데이터 포함)

- 응답(Response)
  ```
  HTTP/1.1 200 OK
  Content-Type: text/html
  Content-Length: 1024

  <html>...</html>
  ```
  - Start Line: HTTP 버전, 상태 코드  
  - Header: 응답 관련 추가 정보  
  - Body: HTML, JSON 등 실제 데이터

<br>

### HTTP Method
| 메서드  | 설명 |
|--------|------|
| `GET` | 데이터 조회 |
| `POST` | 데이터 생성 |
| `PUT` | 데이터 전체 수정 |
| `PATCH` | 데이터 일부 수정 |
| `DELETE` | 데이터 삭제 |

<br>

### HTTP Method 속성
| 메서드  | 안전(Safe) | 멱등(Idempotent) | 캐시 가능(Cacheable) |
|--------|----------|----------------|--------------|
| `GET` | Ο | Ο | Ο |
| `POST` | Χ | Χ | Χ |
| `PUT` | Χ | Ο | Χ |
| `PATCH` | Χ | Χ | Χ |
| `DELETE` | Χ | Ο | Χ |

1. 안전(Safe)  
   - 서버의 데이터를 변경하지 않는 메서드는 안전한 메서드.  
   - `GET`은 데이터를 조회하는 역할만 하므로 안전함.  
   - `POST`, `PUT`, `PATCH`, `DELETE`는 서버 데이터를 변경할 가능성이 있으므로 안전하지 않음.  

2. 멱등(Idempotent)  
   - 동일한 요청을 여러 번 보내도 결과가 변하지 않는다면 멱등한 메서드.  
   - `GET`은 여러 번 호출해도 같은 데이터를 반환하므로 멱등함.  
   - `PUT`과 `DELETE`도 멱등한데, `PUT`은 같은 값을 덮어쓰는 방식이므로 여러 번 실행해도 결과가 같음. `DELETE`는 한 번 실행하든 여러 번 실행하든 동일한 리소스를 삭제하므로 멱등함.  
   - `POST`는 새로운 리소스를 생성하는 요청이므로 멱등하지 않음. `PATCH`도 일부 데이터만 수정하는 요청이므로 여러 번 실행하면 결과가 달라질 수 있어 멱등하지 않음.  

3. 캐시 가능(Cacheable)  
   - 캐시 가능한 메서드는 브라우저나 프록시 서버가 응답을 저장하고, 동일한 요청이 오면 저장된 응답을 반환할 수 있음.  
   - `GET`은 캐시가 가능하며, 성능 최적화에 유용함.  
   - `POST`, `PUT`, `PATCH`, `DELETE`는 일반적으로 캐시되지 않음. 특히 `POST`는 호출할 때마다 새로운 리소스를 생성할 가능성이 있어 캐시되지 않음.

<br>

### HTTP 상태 코드
| 상태 코드 | 의미 |
|----------|------|
| 2xx (성공) | `200 OK`, `201 Created`, `204 No Content` |
| 3xx (리다이렉션) | `301 Moved Permanently`, `302 Found` |
| 4xx (클라이언트 오류) | `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found` |
| 5xx (서버 오류) | `500 Internal Server Error`, `503 Service Unavailable` |

<br>

### HTTP API 설계
| 기능 | HTTP 메서드 | URL |
|------|-----------|-----|
| 게시글 생성 | `POST` | `/boards` |
| 게시글 조회 | `GET` | `/boards/{id}` |
| 게시글 목록 조회 | `GET` | `/boards` |
| 게시글 수정 | `PUT or PATCH` | `/boards/{id}` |
| 게시글 삭제 | `DELETE` | `/boards/{id}` |

설계 원칙
- 리소스 중심 (URI는 명사 사용, 동사 사용 X)
- 복수형 사용 (`board → boards`)
- HTTP 메서드를 활용하여 CRUD 표현

<br>

### HTTP Header
1. 요청 헤더
   | 헤더 | 설명 |
   |------|------|
   | `Host` | 요청 대상 서버 주소 |
   | `User-Agent` | 클라이언트 정보 (브라우저, OS) |
   | `Accept` | 클라이언트가 받을 수 있는 데이터 타입 |

2. 응답 헤더
   | 헤더 | 설명 |
   |------|------|
   | `Content-Type` | 응답 데이터 타입 (`application/json`) |
   | `Set-Cookie` | 쿠키 설정 |
   | `Location` | 리다이렉트 주소 |

3. 캐시 관련 헤더
   | 헤더 | 설명 |
   |------|------|
   | `Cache-Control` | 캐시 정책 (`max-age`, `no-cache` 등) |
   | `ETag` | 캐시 무결성 체크 |
   | `If-Modified-Since` | 마지막 수정 날짜 |

<br>

### Restful API
REST(Representational State Transfer)란?
- HTTP 기반의 API 설계 원칙.
- 리소스를 고유한 URI로 식별하고, HTTP 메서드를 활용하여 조작.

#### RESTful API 설계 규칙
1. URI는 명사 사용, 동사 X
   - ❌ `/getUsers` → ✅ `/users`
2. 복수형 사용
   - ❌ `/user` → ✅ `/users`
3. 슬래시(/) 계층 구조 표현
   - `/users/{id}/posts`
4. CRUD 표현은 HTTP 메서드 활용
   - `GET /users/{id}` (조회)
   - `POST /users` (생성)
   - `PUT /users/{id}` (전체 수정)
   - `PATCH /users/{id}` (일부 수정)
   - `DELETE /users/{id}` (삭제)

#### REST API 성숙도 모델
| 레벨 | 설명 |
|------|------|
| Level 0 | URI만 사용 (`POST /operation`) |
| Level 1 | 리소스 기반 설계 (`POST /users`) |
| Level 2 | HTTP 메서드 활용 (`GET /users/{id}`) |
| Level 3 | HATEOAS 적용 (응답에 다음 단계 링크 포함) |

---

## 웹 애플리케이션(Web Application)

### Web Server
- HTTP 기반으로 동작하며 정적 리소스(HTML, CSS, JS, 이미지 등) 를 제공.
- 정적 리소스: 서버에 저장된 그대로 클라이언트에 전달되는 데이터.
- 대표적인 웹 서버:
  - NGINX
  - Apache

#### Web Server의 역할
1. 정적 리소스 제공
   - HTML, CSS, JS, 이미지, 영상 파일 등의 요청 처리.
2. 요청을 WAS로 전달
   - API 요청 또는 동적인 처리가 필요한 요청을 WAS로 넘김.

<br>

### WAS (Web Application Server)
- HTTP 기반으로 동작하며 웹 서버의 기능 + 애플리케이션 로직 실행 + DB와 연동.
- 동적 콘텐츠 처리 (예: 로그인, 게시글 조회, 회원가입 등).
- 대표적인 WAS:
  - Tomcat (Spring Boot 내장)
  - Jetty
  - Undertow

<br>

### Web Server와 WAS 차이점
|  | Web Server | WAS |
|---|---|---|
| 역할 | 정적 리소스 제공 | 동적 콘텐츠 처리 |
| 예시 | HTML, CSS, JS, 이미지 | DB 조회, 로직 실행 |
| 대표 서버 | NGINX, Apache | Tomcat, Jetty, Undertow |

<br>

### Web System 구성 방식

#### WAS만 사용하는 경우
- 모든 요청(Web 페이지, API)을 WAS가 처리.
- 단점:
  - 과부하 발생 가능성 증가.
  - 정적 리소스 요청도 WAS가 처리하므로 비효율적

#### Web Server + WAS 구성
- Web Server: 정적 리소스 처리.
- WAS: 동적 로직 처리(API 요청).
- 장점:
  - 서버 부담 분산 (정적 리소스는 Web Server, 동적 처리는 WAS).
  - 오류 페이지 제공 가능 (WAS 다운 시 Web Server가 대체 응답).

<br>

### Servlet
- Java 기반 HTTP 요청 및 응답 처리 기술.
- `HttpServlet` 클래스를 상속받아 구현.
- 웹 애플리케이션에서 사용자의 요청을 받고 비즈니스 로직을 실행한 후 응답을 반환.

#### 동작 순서
1. 클라이언트가 요청을 보냄 (`localhost:8080/example`).
2. WAS가 Request, Response 객체 생성.
3. Servlet 실행 (`service()` 메서드 호출).
4. 비즈니스 로직 처리.
5. 응답 작성 후 클라이언트에게 전송.
6. Request, Response 객체 소멸.

#### Servlet Container
- Servlet을 관리하는 환경 (WAS 내부에서 동작).
- 역할:
  - Servlet 객체 생성, 초기화, 호출, 종료 관리.
  - 싱글톤 관리 (Servlet 하나만 생성하여 공유).
  - Multi-Thread 지원 (동시 요청 처리 가능).

<br>

### Thread & Multi-Thread

#### Single Thread 방식
- 한 개의 요청만 처리 가능.
- 문제점: 하나의 요청이 지연되면 다른 요청도 모두 대기.

#### Multi Thread 방식 (WAS 지원)
- 여러 개의 요청을 동시에 처리 가능.
- 요청마다 새로운 Thread 생성하여 처리.
- Thread Pool 방식 사용:
  - 미리 Thread를 생성해두고 요청이 오면 할당.
  - 사용 완료된 Thread는 다시 Pool로 반환.

#### Thread Pool의 장점
1. Thread 생성 비용 절약 → 빠른 응답.
2. 서버 과부하 방지 (최대 Thread 개수 제한).
3. 동시 요청을 효율적으로 처리.

<br>

### SSR(Server Side Rendering)
- 서버에서 HTML을 생성 후 클라이언트에 전달하는 방식.
- Java에서는 JSP, Thymeleaf 등을 사용.

#### 동작 흐름:
1. 클라이언트가 HTML 요청.
2. 서버(WAS)에서 DB 조회 후 HTML 생성.
3. HTML을 클라이언트에게 반환.

#### 장점:
1. 초기 로딩 속도 빠름 (완성된 HTML 전달).
2. SEO(Search Engine Optimization)에 유리 (검색 엔진이 HTML을 크롤링 가능).

#### 단점:
1. 서버 부하 증가 (모든 요청마다 HTML을 생성해야 함).
2. 페이지 전환 시 속도 느림 (서버에서 매번 HTML 생성).

<br>

### CSR(Client Side Rendering)
- 클라이언트(브라우저)에서 HTML을 동적으로 생성하는 방식.
- React, Vue 같은 프레임워크 사용.

#### 동작 흐름:
1. 클라이언트가 빈 HTML을 요청.
2. 브라우저가 JS를 다운로드하여 실행.
3. JS가 HTTP API 요청 → JSON 응답 받음.
4. JSON 데이터를 기반으로 HTML 동적 생성.

#### 장점:
1. 빠른 페이지 전환 (서버와 통신 없이 클라이언트에서 렌더링).
2. 유저 인터랙션(상호작용)에 빠르게 반응.

#### 단점:
1. 초기 로딩 속도 느림 (JS 다운로드 후 실행 필요).
2. SEO에 불리할 수 있음 (검색 엔진이 JS 실행을 못 하면 문제 발생).


---
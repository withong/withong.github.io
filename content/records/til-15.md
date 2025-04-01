+++
title = "Spring 기초 1"
date = "2025-03-17T22:00:03+09:00"
draft = false
topic = ["records"]
tag = ["내일배움캠프", "Spring"]
+++

---

## 네트워크
- 인터넷(Internet)
  - TCP/IP 기반으로 동작하는 글로벌 네트워크.
  - 데이터 전송을 위해 유선(해저 케이블) 및 무선(위성 통신) 활용.
- 인터넷 프로토콜(IP, Internet Protocol)
  - 네트워크 내에서 기기를 식별하는 주소 체계.
  - 예시: `192.168.0.1`
- IP 방식의 문제점
  - 애플리케이션 구분 불가능.
  - 비연결성: 수신 대상의 상태를 고려하지 않고 전송.
  - 비신뢰성: 패킷 손실 및 순서 뒤섞임 가능.

---

### TCP와 UDP
- TCP(Transmission Control Protocol)
  - 신뢰성 있는 데이터 전송 (3-Way Handshake 방식).
  - 패킷 순서 보장 및 오류 검출 가능.
- UDP(User Datagram Protocol)
  - 빠른 전송 속도를 우선시 (비신뢰성 전송).
  - 실시간 스트리밍, 온라인 게임 등에 사용.
- PORT
  - 같은 IP에서 여러 프로세스를 구분하는 번호.
  - 주요 포트 번호: HTTP(80), HTTPS(443), FTP(21), SSH(22).

---

## Web
- DNS(Domain Name System)
  - 도메인과 IP 주소를 매핑하는 시스템.
  - 사람이 이해하기 쉬운 도메인(`google.com`)을 IP 주소로 변환.
- URI(Uniform Resource Identifier)
  - 인터넷 자원의 고유 식별자.
- URL(Uniform Resource Locator)
  - 리소스의 위치를 나타내는 형식 (`https://spartacodingclub.kr/`).

---

### 브라우저에 URL을 입력하면
1. 사용자가 브라우저에 URL을 입력.
2. DNS 조회를 통해 도메인에 해당하는 IP 주소 확인.
3. 웹 브라우저가 HTTP 요청을 생성.
4. 요청 패킷이 서버로 전송.
5. 서버에서 HTTP 요청을 처리 후 응답 생성.
6. 응답 패킷이 클라이언트로 전송.
7. 브라우저가 응답 데이터를 렌더링하여 화면 표시.

---

## 용어 모음
- 프로그래밍 명명규칙(Casing)
  - `snake_case`: Python, DB 테이블에서 사용 (`user_name`).
  - `camelCase`: Java, JavaScript에서 변수 및 메서드 이름에 사용 (`userName`).
  - `PascalCase`: 클래스 및 객체 이름에 사용 (`UserName`).
  - `kebab-case`: URL 및 CSS에서 사용 (`user-name`).
- JSON(JavaScript Object Notation)
  - 클라이언트-서버 간 데이터 교환을 위한 경량 데이터 포맷.
  - XML 대비 가독성이 뛰어나고 용량이 작음.
- Scale Up, Scale Out
  - Scale Up: 단일 서버의 성능 향상 (CPU, RAM 증가).
  - Scale Out: 여러 대의 서버 확장 (부하 분산).
- Stateful vs Stateless
  - Stateful: 클라이언트의 상태를 유지해야 하는 서비스.
  - Stateless: 서버가 클라이언트 상태를 저장하지 않음 (확장성 우수).
- Connection vs Connectionless
  - Connection: 지속적 연결 유지 (HTTP/1.0).
  - Connectionless: 필요할 때만 연결 (HTTP/1.1, HTTP/2).

---

## HTTP
- HTTP(HyperText Transfer Protocol)
  - 인터넷 상에서 클라이언트와 서버 간 데이터를 교환하는 프로토콜.
- HTTP 특징
  - Stateless(무상태): 클라이언트의 상태를 저장하지 않음.
  - Connectionless(비연결): 요청-응답 후 연결을 끊음.
- HTTP Message 구조
  - Request (요청)
    - Start Line: HTTP Method, URL, HTTP Version.
    - Header: 추가적인 요청 정보 (예: `Content-Type`).
    - Body: 요청 데이터 포함 (POST 요청 등).
  - Response (응답)
    - Status Code: 요청의 처리 결과 (`200 OK`, `404 Not Found` 등).
    - Header: 응답 메타데이터.
    - Body: 응답 데이터.

---

### HTTP Method와 상태 코드
- HTTP Method
  - `GET`: 데이터 조회.
  - `POST`: 데이터 생성.
  - `PUT`: 데이터 전체 수정.
  - `PATCH`: 데이터 일부 수정.
  - `DELETE`: 데이터 삭제.
- HTTP 상태 코드
  - `200 OK`: 정상 응답.
  - `201 Created`: 리소스 생성 완료.
  - `400 Bad Request`: 잘못된 요청.
  - `401 Unauthorized`: 인증 필요.
  - `404 Not Found`: 리소스 없음.
  - `500 Internal Server Error`: 서버 내부 오류.

---

### HTTP Header
- HTTP Header의 역할
  - 요청과 응답의 부가 정보 제공.
- 자주 사용하는 Header
  - `Content-Type`: 데이터 형식 지정 (`application/json`).
  - `Authorization`: 인증 정보 포함.
  - `Cache-Control`: 캐시 정책 (`max-age`, `no-cache` 등).
  - `Set-Cookie`: 쿠키 설정 (세션 관리).

---

## Restful API
- REST (Representational State Transfer)
  - 리소스를 기반으로 한 API 설계 원칙.
- RESTful API 설계 예시
  - `POST /boards` → 게시글 생성.
  - `GET /boards` → 게시글 목록 조회.
  - `GET /boards/{id}` → 특정 게시글 조회.
  - `PUT /boards/{id}` → 게시글 수정.
  - `DELETE /boards/{id}` → 게시글 삭제.

---

## Web Application
- Web Server vs WAS(Web Application Server)
  - Web Server: 정적 콘텐츠 제공 (예: Nginx, Apache).
  - WAS: 동적 콘텐츠 처리 (예: Tomcat, Spring Boot).
- Web System 구성
  - 프론트엔드(클라이언트) → 백엔드(WAS) → 데이터베이스(DB).

---

### Servlet
- Servlet
  - Java 기반 웹 애플리케이션을 처리하는 기술.
- Servlet 동작 순서
  1. 클라이언트가 요청 전송.
  2. Servlet Container가 요청을 분석하여 Servlet 실행.
  3. Servlet이 요청을 처리하고 응답 생성.
  4. 응답이 클라이언트에게 전달됨.

---

### Thread
- Thread
  - 하나의 프로세스 내에서 실행되는 작업 단위.
- Multi-Thread
  - 여러 작업을 동시에 실행하는 기술 (서버 성능 향상 가능).

---

### SSR(Server Side Rendering) vs CSR(Client Side Rendering)
- SSR: 서버에서 HTML을 생성하여 클라이언트로 전송 (초기 로딩 속도 빠름).
- CSR: 클라이언트에서 JavaScript를 이용해 동적으로 렌더링 (SPA에서 주로 사용).

---
+++
title = "Spring 심화 2"
date = "2025-03-28T20:28:44+09:00"
draft = false
topic = ["record"]
tag = ["내일배움캠프", "Spring"]
+++

---

## 인증(Authentication)과 인가(Authorization)

| 용어 | 의미 | 예시 |
|------|------|------|
| 인증 | 사용자가 **누군지 확인하는 절차** | 로그인 (ID, PW 입력 후 사용자 확인) |
| 인가 | 인증된 사용자가 **무엇을 할 수 있는지 확인** | 본인이 쓴 글만 수정 가능, 관리자만 삭제 가능 |

→ 인가는 **항상 인증이 먼저** 되어야 가능

---

## Cookie

### 쿠키란?

- 클라이언트(브라우저)에 저장되는 작은 데이터
- 서버는 `Set-Cookie`라는 응답 헤더로 쿠키를 전달
- 이후 클라이언트는 모든 요청에 `Cookie` 값을 함께 보냄

### 왜 필요할까?

HTTP는 기본적으로 **Stateless(상태를 기억하지 않음)** 이라서  
서버는 사용자가 누군지 매 요청마다 모르게 됨.  

→ 예: 로그인한 사용자인지 아닌지 모르니까, 매번 인증정보를 넣어야 함  
→ 그래서 사용자의 식별자 같은 정보를 **쿠키에 저장**해서 매번 같이 보냄

---

### 쿠키 구조

```http
Set-Cookie: userId=1; Max-Age=3600; Path=/; Domain=example.com; HttpOnly; Secure
```

- `userId=1`: 키-값
- `Max-Age=3600`: 1시간 동안 유지 (없으면 브라우저 종료 시 삭제 = 세션 쿠키)
- `HttpOnly`: JS 접근 차단 (XSS 방지)
- `Secure`: HTTPS 통신에서만 쿠키 전송
- `Path=/`: 어느 경로에서 쿠키를 사용할지

### 쿠키의 보안 문제

- 쿠키는 클라이언트가 **임의로 조작 가능**
  - 브라우저 콘솔 → `document.cookie` → 값 수정 가능
- 탈취될 경우 누구든 userId를 바꾸고 접근 가능
- **민감 정보(userId, 주민번호 등)는 절대 저장 금지**

---

## Session

### 세션이란?

- 쿠키 방식의 보안 문제를 보완하기 위해 사용
- **서버가 상태를 저장**하고, 클라이언트는 식별값만 가지고 있음

### 동작 흐름

1. 로그인 성공 시, 서버가 랜덤한 `sessionId`를 생성
2. 이 `sessionId`와 로그인한 사용자 정보를 서버 메모리에 저장
3. 서버는 클라이언트에게 `Set-Cookie: JSESSIONID=abc123` 전송
4. 이후 클라이언트가 요청할 때 `JSESSIONID`를 함께 전송
5. 서버는 메모리에 저장된 세션을 보고 사용자 정보를 확인

> 사용자는 쿠키에 단순한 세션ID만 갖고 있고, **실제 정보는 모두 서버에 있음**

---

### 세션 문제점

- 서버 메모리를 사용 → 로그인 유저가 많아지면 메모리 터짐
- 서버가 여러 대일 경우, 세션 공유가 어려움 (Scale-Out의 한계)
- 세션 타임아웃 후 자동 로그아웃 (기본 30분)
- 모바일 앱이나 외부 API에는 불편

---

## Token & JWT (JSON Web Token)

### Token이란?

- **클라이언트가 직접 가지고 있는 인증 정보**
- 문자열로 이루어진 인증 수단
- Cookie/Session 방식보다 **Stateless**해서 확장성이 뛰어남

### JWT 구조
```
Header.Payload.Signature
```
- **Header**: 토큰의 정보 (알고리즘, 타입 등)
- **Payload**: 사용자 정보(Claims), 토큰 유효기간 등
- **Signature**: 위의 두 개를 기반으로 암호화한 서명 (변조 방지용)

#### 예시

```json
Header: { "alg": "HS256", "typ": "JWT" }
Payload: { "sub": "1", "name": "Spring", "exp": 1710000000 }
Signature: HMACSHA256( base64Url(header) + "." + base64Url(payload), secret )
```

> Header와 Payload는 **Base64로 인코딩**만 되어 있어서, **누구나 복호화 가능**  
→ **민감 정보 저장 금지**  

> Signature는 서버만 아는 secret key로 암호화  
→ 변조 방지  

---

### JWT 장점 vs 단점

| 장점 | 단점 |
|------|------|
| DB 없이 유저 정보 포함 가능 | Payload가 암호화되지 않음 |
| Scale-Out에 유리 | Token 탈취되면 대처 어려움 |
| 세션 없이 인증 처리 가능 | Token 길이로 인한 트래픽 부담 |

---

### Access Token & Refresh Token

- **Access Token**: API 요청에 사용하는 JWT
- **Refresh Token**: Access Token이 만료됐을 때 다시 발급받기 위한 Token

→ Refresh Token은 보통 DB에 저장하고 관리  
→ Access Token 유효기간은 짧게 (보안), Refresh Token은 상대적으로 길게

---

## Servlet Filter

### 필터란?

- 모든 요청(Request)에 대해 **가장 먼저 가로채는 중간 처리기**
- 인증, 인코딩, 로깅 등 **공통 관심사(Cross-cutting concerns)** 처리에 사용

```text
클라이언트 → [Filter] → [DispatcherServlet] → [Controller]
```

---

### 필터 구현 방법

1. `Filter` 인터페이스 구현
2. `doFilter()` 메서드에서 필터링 로직 작성
3. `FilterRegistrationBean`으로 필터 등록

```java
@Configuration
public class WebConfig {
    @Bean
    public FilterRegistrationBean<LoginFilter> loginFilter() {
        FilterRegistrationBean<LoginFilter> registration = new FilterRegistrationBean<>();
        registration.setFilter(new LoginFilter());
        registration.setOrder(1);
        registration.addUrlPatterns("/*");
        return registration;
    }
}
```

> `setOrder()`는 실행 순서 (낮을수록 먼저 실행)

---

### 실습 예시

- `/home` 접근 시 Cookie에 `userId` 확인
- 없으면 로그인 페이지로 redirect
- 로그인 성공 시 userId 쿠키 생성
- `Filter`에서 특정 URL 이외 요청에는 로그인 상태 확인
- 로그인 정보 없으면 예외 발생

---

## 핵심 정리

| 항목 | 설명 |
|------|------|
| Cookie | 클라이언트 저장, 보안 취약 |
| Session | 서버 저장, 메모리 사용 |
| Token | 클라이언트 저장, Stateless |
| JWT | Token 포맷, 변조 방지에 강함 |
| Filter | 공통 처리 로직 (인증, 로깅 등) |

---


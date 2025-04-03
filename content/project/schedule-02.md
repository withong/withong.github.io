+++
title = "일정 관리 API (JPA) 트러블 슈팅"
date = "2025-04-03T22:28:58+09:00"
draft = false
tag = ["내일배움캠프", "일정 관리 API", "트러블 슈팅"]
+++

---
## GitHub
### ▶ [nbc-chapter-03-develop](https://github.com/withong/nbc-chapter-03-develop.git)

<hr>
<br>

## 1. `@Email`로 검증했는데 이상한 이메일도 통과되는 문제

- **문제**:  
  `@Email`을 써도 `test@gmail` 같은 이메일이 통과됨...  

- **원인**:  
  `@Email`은 기본적으로 `@` 기호와 텍스트 구조만 확인하고,  
  도메인에 `.`이 있어야 한다는 조건은 강제하지 않음  

- **분석**:  
  `@Email`만으로는 `"@뒤에 .이 있어야 함"` 같은 현실적인 이메일 형식 검증은 부족함  

- **해결**:  
  정규 표현식을 함께 사용해 `@` 뒤에 `.` 포함 여부와 `.` 뒤의 문자열까지 추가로 검증  

▶ 공부 기록 [[정규 표현식을 활용한 사용자 등록 요청 데이터 검증]](https://withong.github.io/record/2025-04-01-02/)

<br>
<hr>
<br>

## 2. 해시된 비밀번호가 매번 다른데, 어떻게 비교해서 로그인하는지?

- **문제**:  
  bcrypt는 매번 다른 해시값을 생성하는데, 그러면 로그인 시 어떻게 비밀번호를 비교함?  

- **원인**:  
  bcrypt의 내부 구조(salt 포함, 비교 로직 내장)에 대한 이해 부족  

- **분석**:  
  bcrypt는 해시값 안에 salt, cost, 알고리즘 정보를 전부 포함하고 있음  

- **해결**:  
  `BCrypt.verifyer().verify()`를 사용하면 입력된 비밀번호와 기존 해시값을 내부적으로 비교 가능함  

▶ 공부 기록 [[Bcrypt를 활용한 비밀번호 암호화]](https://withong.github.io/record/2025-04-01-03/)

<br>
<hr>
<br>

## 3. Filter에서 발생한 예외가 ExceptionHandler로 전달되지 않음

- **문제**:  
  `Filter`에서 `throw new CustomException(...)` 했는데,  
  예외 핸들러가 작동하지 않고 500 에러만 발생

- **원인**:  
  `Filter`는 Spring MVC 흐름 바깥이라 `@ExceptionHandler`가 작동하지 않음

- **분석**:  
  `DispatcherServlet` 이전 단계에서 예외가 발생하면  
  Spring의 예외 처리 체계까지 도달하지 않기 때문

- **해결**:  
  `sendError()` 대신, `ObjectMapper`로 JSON 응답을 직접 작성해서  
  `response.getWriter().write(json)` 방식으로 내려줌  

```java
if (session == null || session.getAttribute(Const.LOGIN_USER) == null) {
    // 사용자 인증 실패 시 응답 반환 처리
    
    // 사용자 정의 예외 코드
    ExceptionCode code = ExceptionCode.NOT_LOGINED;

    // 사용자 정의 예외 응답 객체
    ExceptionResponse responseBody = ExceptionResponse.builder()
            .status(code.getStatus().value())
            .code(code.getCode())
            .message(code.getMessage())
            .build();

    httpResponse.setStatus(code.getStatus().value());   // 응답 상태 코드 설정
    httpResponse.setContentType("application/json");    // 응답 타입 설정 (JSON)
    httpResponse.setCharacterEncoding("UTF-8");         // 응답 인코딩 설정

    String json = new ObjectMapper().writeValueAsString(responseBody); // 객체를 JSON 문자열로 변환
    httpResponse.getWriter().write(json); // 응답 바디에 JSON 문자열 작성

    return;
}
```

<br>
<hr>
<br>

## 4. 본인 일정은 본인만 볼 수 있음 vs 댓글 기능 요구사항 충돌

- **문제**:  
  본인 일정만 볼 수 있게 했는데, 댓글 기능을 추가하라 하니 구조상 말이 안 됨  
  나만 보는 내 일정에 나만 보는 댓글을 나만 달 수 있다...(?)

- **원인**:  
  "본인 일정은 본인만 조회"로 설계해둔 상태에서 다른 사용자가 댓글을 달 수 없음

- **분석**:  
  댓글 기능이 의미 있으려면 **일정 조회 권한**을 열어야 함  
  → 최소한 조회는 공개, 수정/삭제만 본인만 가능해야 함

- **해결**:  
  일정 조회 API를 **타인도 접근 가능하게 수정**,  
  전체 사용자 조회 → 특정 사용자 일정 조회 흐름으로 확장 설계  

---

## 5. 로그인 상태에서 다른 계정으로 중복 로그인 시도하면?

- **문제**  
  동일한 브라우저에서 A 탭으로 A 계정 로그인, B 탭으로 B 계정 로그인 시  
  세션 충돌 또는 로그아웃은 어떻게 처리되는가?

- **원인**  
  Spring에서 로그인 시 `HttpSession`은 클라이언트의 브라우저 쿠키(`JSESSIONID`) 기준으로 관리됨  
  즉, **브라우저 단위로 세션 1개**만 유지됨

- **분석**  
  B 탭에서 로그인하면, 같은 브라우저니까 같은 세션에 `LOGIN_USER`가 덮어씌워짐  
  A 탭은 여전히 로그인된 상태로 보이지만, 실제로는 **이미 B로 덮인 세션 사용 중**  
  A 탭에서 B의 본인 정보 조회 및 정보 수정 접근 가능해짐

- **해결**  
  로그인 전 기존 세션 무효화 처리 추가:  

  ```java
  HttpSession existingSession = httpRequest.getSession(false);
  if (existingSession != null) {
      existingSession.invalidate();
  }
  ```

---

## 6. QueryDSL 페이징 시 count 쿼리가 왜 따로 필요한지?

- **문제**  
  페이징 구현에서 `.offset`, `.limit`만으로 잘 동작하는데  
  왜 굳이 `count 쿼리`를 따로 날려야 하는 건지?

- **원인**  
  Spring Data JPA의 `Page` 객체는 전체 페이지 수 계산을 위해 **총 데이터 개수(totalElements)** 필요

- **분석**  
  단순히 일부 데이터만 잘라서 가져오면 현재 페이지까지만 보여줄 수 있음  
  하지만 프론트에서 "총 몇 페이지인지", "다음 페이지 있는지"를 렌더링하려면 **총 개수 필수**

- **해결**  
  `PageImpl<>(content, pageable, totalCount)` 형태로  
  `content` + `pageable` + `count 쿼리`까지 포함해서 `Page` 객체 반환

---

### 5. 일정 목록/단건 조회의 댓글 개수 처리 로직 분리

- **문제**  
  일정 목록 조회에도 일정 단건 조회에도 일정별 댓글 개수를 보여주고 싶은데,  
  일정별 댓글 개수 조회를 `countCommentsBySchedule(schedule)` 하나로 처리하려니  
  일정 목록을 조회할 때마다 일정 개수만큼 댓글 개수 쿼리가 반복 실행됨

- **원인**  
  `countCommentsBySchedule`는 단일 일정의 댓글 개수를 세는 쿼리인데,  
  목록 전체에 대해 이 메서드를 반복 호출하면 **N+1 문제**처럼 작동해서 쿼리가 너무 많아짐.

- **분석**  
  - 일정 목록을 조회하면서 일정별 댓글 개수도 **같은 쿼리에서 함께 조회**하는 게 효율적임.

- **해결**  
  - **일정 목록 조회 시**: `Querydsl`을 사용해 일정 목록을 조회할 때 `comment.count()`를 `groupBy(schedule.id)`와 함께 조회함으로써 댓글 개수를 포함한 결과를 한 번의 쿼리로 가져오도록 처리함.
  - **일정 단건 조회 시**: `CommentRepository.countCommentsBySchedule(schedule)` 메서드를 그대로 활용해서, 단일 일정에 대한 댓글 개수만 별도로 조회.

▶ 공부 기록 [[QueryDSL을 활용한 조건 기반 일정 조회]](https://withong.github.io/record/2025-04-03/)

<br>
<hr>
+++
title = "일정 관리 API (JDBC) 트러블 슈팅"
date = "2025-03-25T20:20:11+09:00"
draft = false
tag = ["내일배움캠프", "일정 관리 API", "트러블 슈팅"]
+++

---
## GitHub
### ▶ [nbc-chapter-03-basic](https://github.com/withong/nbc-chapter-03-basic.git)

---

<br>

## 1. 등록 시 createdAt, updatedAt Null
createdAt, updatedAt는 DB에서 자동 갱신됨

### 1.1. 문제
일정/사용자 등록 후 반환된 createdAt, updatedAt 값이 Null

### 1.2. 원인
당연함... 등록 요청 시 전달받은 정보를 응답 DTO에 넣어주고 그 값을 반환하는데  
DB에서 자동으로 업데이트 하는 값은 안 들어오니까 안 넣어줬고...  
DB 조회해서 가져오기 전까지 당연히 Null임.

### 1.3. 해결
전달받은 식별자를 통해 재 조회한 결과 반환

<br>
<hr>
<br>

## 2. 변경 요청 시 변경된 내용이 없을 경우
### 2.1. 문제
클라이언트가 기존 정보와 동일한 정보로 변경 요청 시  
jdbcTempalte.update의 반환 값이 0이 올 것이라 예상했으나  
예상 결과와 다르게 동일한 값도 update 후 1이 반환됨  

>jdbcTemplate.update(...)의 반환값: 영향받은 row의 개수

### 2.2. 원인
동일한 값이어도 query의 문법 오류는 없으므로 실행됨

### 2.3. 해결
repository에 전달하기 전에 변경 전 정보를 변수로 기억함.  
변경 후의 정보와 비교 후 동일한 값이면 204 No Content 응답 반환.

#### Q...
프론트에서 클라이언트에게 보여줄 메시지를 위해 해야 할 고민이었을까?  
하지만 데이터 관련 문제니까 백엔드에서 하는 게 맞는 것 같기도?

<br>
<hr>
<br>

## 3. DTO 목적별 분리
### 3.1. 문제
유효성 검증을 적용해야 하는데 등록할 때와 수정할 때의 필수 입력 값이 다름.  
등록할 때에는 이름과 이메일이 필수 입력 값이지만, 수정할 때에는 둘 다 Null이 가능함.

### 3.2. 원인
하나의 requestDto를 등록/수정에서 같이 사용하고 있기 때문.

### 3.3. 해결
DTO를 목적별로 분리하고 각 목적에 맞는 유효성 검증을 적용함.

<br>
<hr>
<br>

## 4. GET 요청 시 유효성 검증 실패 예외 메시지
### 4.1. 문제
@RequestParam에 @NotNull(message = "...") 같이 유효성 어노테이션을 붙였을 때,  
내가 작성한 예외 메시지 앞에 메서드 경로 정보가 포함되어 전달됨...  
```json
"message": "findSchedulesWithUserByUserId.userId: userId는 필수 입력값입니다."
```

### 4.2. 원인
@RequestParam + @Validated 조합에서는 내부적으로 ConstraintViolationException이 발생함. 이 예외 객체 안의 message는 `<메서드이름>.<파라미터이름>: <어노테이션 message>`와 같이 구성됨.  

### 4.3. 해결
파라미터들을 DTO로 묶은 후 @ModelAttribute로 받는 방식으로 변경
```java
/**
 * 일정 목록 조회 요청 DTO
 */
@Setter
@Getter
@NoArgsConstructor
public class ScheduleQueryRequestDto {

    /**
     * 사용자 식별자 (필수)
     */
    @NotNull(message = "사용자 ID는 필수 값입니다.")
    private Long userId;
    ...
}
```

<br>
<hr>
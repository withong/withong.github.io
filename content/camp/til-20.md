+++
title = "[내일배움캠프] 일정 관리 서버 트러블 슈팅"
date = "2025-03-25T20:20:11+09:00"
draft = ture
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "일정_관리_서버", "트러블_슈팅"]
+++

---
## GitHub
### ▶ [nbc-chapter-03-basic](https://github.com/withong/nbc-chapter-03-basic.git)

---

## 1. 등록 시 createAt, updatedAt Null
createAt, updatedAt는 DB에서 자동 갱신됨

### 1.1. 문제
일정/사용자 등록 후 반환된 createAt, updatedAt 값이 Null

### 1.2. 원인
당연함... 
등록 요청 시 전달받은 정보를 응답 DTO에 넣어주고 그 값을 반환하는데  
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
예상 결과와 다르게 같은 값도 update 후 1이 반환됨  

#jdbcTemplate.update(...)의 반환값: 영향받은 row의 개수

### 2.2. 원인
동일한 값이어도 query의 문법 오류는 없으므로 실행됨

### 2.3. 해결
repository에 전달하기 전에 변경 전 정보를 변수로 기억함.  
변경 후의 정보와 비교 후 동일한 값이면 204 No Content 응답 반환.

#### Q...
프론트에서 클라이언트에게 보여줄 메시지를 위해 해야 할 고민이었을까?  
하지만 데이터 관련 문제니까 백엔드에서 하는 게 맞는 것 같기도?

---
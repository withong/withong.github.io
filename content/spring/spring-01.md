+++
title = "Spring Data JPA"
date = "2025-03-30T19:24:21+09:00"
draft = true
topic = ["spring"]
tag = ["study", "spring", "JPA"]
+++

---

과제 진행하면서 공부한 개념들 정리...  

---
## Q
등록일, 수정일을 왜 BaseEntity로 나누어서 관리해야 하는지?  
BaseEntity class 위에만 붙는 어노테이션들의 의미와 역할은 뭔지?  

---

### @MappedSuperclass & @EntityListeners(AuditingEntityListener.class)  
JPA에서 공통 엔티티 속성(등록일, 수정일 등)을 상속 구조로 관리할 때 꼭 사용되는 핵심 요소  

#### @MappedSuperclass
공통 매핑 정보만 물려주는 부모 클래스

**역할**
이 어노테이션이 붙은 클래스는 JPA 엔티티가 아니고, 공통 필드만 자식 클래스에게 전달해주는 용도  
DB에 별도로 테이블이 생성되지 않음  
이 클래스를 상속받는 엔티티 클래스에 필드만 전달됨  

**언제 쓰는가?**
createdAt, modifiedAt, isDeleted, createdBy 같은 모든 엔티티에 반복되는 공통 필드가 있을 때  
그 공통 필드를 묶은 클래스를 만들어두고 상속해서 재사용하고 싶을 때  

<br>

#### @EntityListeners
이 엔티티에 어떤 이벤트 리스너를 붙일지 지정하는 어노테이션

JPA에서는 다음과 같은 **"엔티티 생명주기 이벤트"**가 있어요:
@PrePersist: 저장(insert) 직전에 실행
@PostPersist: 저장 직후
@PreUpdate: 업데이트 직전에 실행
@PostUpdate: 업데이트 직후
@PreRemove, @PostRemove, @PostLoad 등
이런 이벤트들이 발생할 때, 자동으로 특정 메서드를 실행하게 해주는 게 **"엔티티 리스너(Entity Listener)"**예요.

#### AuditingEntityListener.class
Spring Data JPA에서 제공하는 감시 전용 리스너 클래스
@CreatedDate, @LastModifiedDate, @CreatedBy, @LastModifiedBy 같은 어노테이션이 붙은 필드들을 감지해서

엔티티가 저장되거나 수정될 때 자동으로 값을 넣어주는 역할을 해요.

---
@CreatedDate, @LastModifiedDate 사용을 위해
@EnableJpaAuditing 설정 등록

---

첫 실행 create table

Hibernate: 
    create table schedules (
        date date not null,
        created_at datetime(6) not null,
        id bigint not null auto_increment,
        updated_at datetime(6) not null,
        user_id bigint not null,
        content TEXT not null,
        primary key (id)
    ) engine=InnoDB
Hibernate: 
    create table users (
        created_at datetime(6) not null,
        id bigint not null auto_increment,
        updated_at datetime(6) not null,
        email varchar(255) not null,
        name varchar(255) not null,
        password varchar(255) not null,
        primary key (id)
    ) engine=InnoDB
Hibernate: 
    alter table schedules 
       add constraint FKd4y4xekwahv9boo6lc8gfl3jv 
       foreign key (user_id) 
       references users (id)

---

GET은 "문자열 ➝ 날짜" 변환이 필요하니까 → @DateTimeFormat
POST는 "JSON ➝ 날짜" 변환이 필요하니까 → @JsonFormat

요청 방식	데이터 형태	변환 대상	사용하는 어노테이션
GET	String (쿼리 파라미터)	문자열 → LocalDate 등	@DateTimeFormat
POST	JSON (Request Body)	JSON → LocalDate 등	@JsonFormat

---

 세션 관련 메서드 정리
메서드	설명
getSession()	세션이 없으면 새로 만들어서 반환 (true가 기본값)
getSession(false)	세션이 있으면 반환, 없으면 null 반환
setAttribute(String, Object)	세션에 값을 저장
getAttribute(String)	세션에서 값을 꺼냄
invalidate()	세션을 완전히 삭제 (로그아웃 시 사용)

---

파라미터에 HttpServletRequest를 넣으면?
```java
@PostMapping("/login")
public ResponseEntity<LoginResponse> login(@RequestBody @Valid LoginRequest request,
                                           HttpServletRequest httpRequest)
```
Spring MVC가 현재 들어온 요청(HttpServletRequest) 객체를 자동으로 넘겨줘요.
이 객체는 사용자 한 명의 요청 전체를 나타내는 객체예요.
여기엔 요청 본문, 헤더, URI, 세션 등 모든 정보가 들어 있어요.

---

httpRequest.getSession()을 호출하면?
```java
HttpSession session = httpRequest.getSession();

```
그 사용자(브라우저)의 요청에 연결된 세션이 있는지 확인

있으면 가져오고

없으면 새로 생성

즉, "서버 쪽에 사용자 전용 저장 공간"을 만들거나 꺼내는 동작이에요.

---



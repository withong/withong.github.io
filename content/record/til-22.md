+++
title = "Spring 심화 1"
date = "2025-03-27T20:25:30+09:00"
draft = false
topic = ["TIL"]
tag = ["내일배움캠프", "Spring"]
+++

---

## 1. Spring Container & Bean

### 1.1. Spring Container
Spring 애플리케이션에서 객체(Bean)를 생성, 관리, 소멸하는 역할을 담당함.
- 설정 파일이나 Annotation을 읽어 Bean을 생성하고 주입하는 과정을 컨트롤함.
- Java에서는 개발자가 객체를 직접 생성하지만, Spring에서는 Container가 관리함.

**Spring Container의 역할**
- Bean 생성, 관리, 의존성 주입을 담당함.
- 객체 간 결합도를 낮추어 OCP, DIP 원칙을 지킬 수 있음.

**Spring Container의 종류**
- BeanFactory: Spring Container의 최상위 인터페이스
- ApplicationContext: BeanFactory를 확장하여 다양한 기능 제공 (국제화, 환경변수, 이벤트, 리소스 조회 등)
- 실무에서는 ApplicationContext를 주로 사용함 → 보통 이것을 Spring Container라고 부름

### 1.2. Spring Bean
Spring Container가 관리하는 객체를 의미함.
- 일반 Java 객체에 Spring이 관리 권한을 부여하면 Bean이 됨.

**Spring Bean의 특징**
1. Container에 의해 생성되고 관리됨
2. 기본적으로 Singleton
3. DI를 통해 다른 Bean과 의존관계를 가질 수 있음
4. 생명주기(생성 → 초기화 → 사용 → 소멸)를 가짐

**Bean 등록 방법**
- XML 방식 (과거)
- Java Annotation (@Component, @Service 등)
- Java 설정 파일 (@Configuration + @Bean)

```java
// Annotation 방식 예시
@Component
public class MyService { }

@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

---

## 2. 객체 제어 원리 (IOC / DI)

### 2.1. IOC (Inversion of Control)
객체의 생성과 관리를 Spring Container가 담당하는 개념
- 제어의 주체가 개발자에서 컨테이너로 바뀜
- 요리를 원하는 사람(개발자)이 직접 요리하지 않고, 요리사(Container)가 대신 요리해주는 개념

### 2.2. DI (Dependency Injection)
Container가 객체 간의 의존성을 자동으로 주입해주는 개념
- 개발자는 구현체를 직접 생성하지 않고, 인터페이스만 의존함
- Spring이 객체 생성 → 주입까지 알아서 해줌

```java
@Service
public class MyServiceImpl implements MyService {
    private final MyRepository myRepository;

    @Autowired
    public MyServiceImpl(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```

- DI 방식: 생성자 주입, Setter 주입, 필드 주입 등이 있음
- 필드 주입은 테스트 어려움, 순환 참조 발생 가능성으로 인해 실무에서는 권장되지 않음

---

## 3. Bean 등록 방식

### 3.1. 자동 등록 - @ComponentScan
지정된 패키지에서 @Component, @Service, @Repository, @Controller 등을 찾아 자동 등록함
- 이 어노테이션들은 모두 @Component를 메타 어노테이션으로 포함하고 있음

```java
@ComponentScan(basePackages = "com.example")
public class AppConfig { }
```

- 스캔 범위는 보통 애플리케이션의 루트 패키지부터 시작함
- Spring Boot의 @SpringBootApplication은 @ComponentScan 포함

### 3.2. 수동 등록 - @Configuration, @Bean
개발자가 명시적으로 Java 설정 파일에 Bean 등록

```java
@Configuration
public class AppConfig {
    @Bean
    public MyService myService() {
        return new MyServiceImpl();
    }
}
```

### 3.3. Bean 충돌
- 자동 vs 자동: 같은 이름의 Bean이 여러 개 존재하면 충돌 발생 → 예외 발생
- 자동 vs 수동: 수동 등록이 자동 등록을 오버라이드함
- Spring Boot에서는 기본적으로 충돌 시 오류 발생

application.properties에서 설정 가능:
```
spring.main.allow-bean-definition-overriding=true
```

---

## 4. 의존관계 설정

### 4.1. 의존관계 주입
@Autowired로 객체의 의존성을 주입받음
- 생성자 주입 (권장), Setter 주입, 필드 주입

```java
@Component
public class MyApp {
    private final MyService myService;

    @Autowired
    public MyApp(MyService myService) {
        this.myService = myService;
    }
}
```

### 4.2. 생성자 주입
- 불변성 보장, 테스트 용이성, 실수 방지
- 생성자가 하나면 @Autowired 생략 가능
- final 필드에만 사용 가능

### 4.3. @RequiredArgsConstructor
Lombok이 final 필드를 기반으로 생성자 자동 생성해주는 Annotation

```java
@RequiredArgsConstructor
@Component
public class MyApp {
    private final MyService myService;
}
```

---

## 5. Bean 선택 충돌 해결

### 5.1. @Primary
같은 타입의 Bean이 여러 개일 때 우선순위 부여

```java
@Component
@Primary
public class MyServiceImplV1 implements MyService { }
```

### 5.2. @Qualifier
구분자 이름을 명시적으로 지정해 특정 Bean을 선택함

```java
@Autowired
public MyApp(@Qualifier("firstService") MyService myService) {
    this.myService = myService;
}
```

---

## 6. Singleton Pattern

### 6.1. Singleton 개요
인스턴스를 하나만 생성하여 공유하는 디자인 패턴
- 메모리 절약, 동일 객체 사용

### 6.2. Spring의 Singleton Bean
- Spring Bean은 기본적으로 Singleton
- 수동 Singleton 구현 방식의 복잡함과 단점들을 Spring이 해결

### 6.3. Stateful Bean 주의
Bean은 무상태(stateless)로 설계해야 함
- 상태를 가진 Bean은 동시성 문제 유발 가능

```java
public class StatefulSingleton {
    private int value; // ❌ 상태를 가지면 안됨
}
```

---

## 7. Validation

### 7.1. BindingResult
검증 오류 정보를 보관하는 객체. Controller에서 유효성 검증 결과를 확인할 수 있음.
- `@ModelAttribute` 바로 뒤에 위치해야 함
- 필드 오류가 있어도 컨트롤러 호출 가능
- 오류 정보는 `bindingResult.getAllErrors()`로 확인

### 7.2. @ModelAttribute vs @RequestBody
요청 데이터를 바인딩하는 방식의 차이
- `@ModelAttribute`: 필드 단위 바인딩, 일부 실패해도 컨트롤러 호출 가능
- `@RequestBody`: JSON 전체 파싱, 실패 시 컨트롤러 호출 불가 (BindingResult로 에러 처리 X)
- `BindingResult` 활용하여 JSON 오류 응답 처리 가능

### 7.3. Bean Validation
필드의 유효성을 Annotation으로 검증하는 표준 방식
- `@NotBlank`, `@NotNull`, `@Range` 등 사용
- `@Valid`, `@Validated`와 함께 사용
- `spring-boot-starter-validation` 의존성 필요

### 7.4. Field Error
개별 필드의 검증 오류 정보
- `bindingResult.getFieldError()` 등으로 확인 가능
- `@ModelAttribute` 사용 시 부분 바인딩 가능

### 7.5. Validator
검증 로직을 수행하는 객체
- `LocalValidatorFactoryBean`이 글로벌 Validator로 자동 등록
- 타입 변환 실패한 필드는 Bean Validation 적용되지 않음

### 7.6. Error Message
에러 메시지는 기본 메시지 또는 커스터마이징 가능
- `@NotBlank(message = "메시지")` 직접 설정
- MessageSource 활용으로 다국어 지원
- Spring의 `DefaultMessageCodesResolver`로 메시지 코드 생성

### 7.7. Object Error
객체 간 관계 또는 전체 검증 오류
- `@ScriptAssert` 가능하나, 자바 코드 방식 권장
- 예: 총합(price * count) 검증 → `bindingResult.reject()` 사용

### 7.8. Bean Validation의 충돌
등록/수정 요청마다 유효성 요구가 다를 때 발생
- 하나의 DTO로 처리 시 상황별 제약 충돌 가능

### 7.9. groups
상황별로 다른 유효성 검증을 적용할 때 사용
- 그룹 인터페이스 정의 후, `@Validated(Group.class)` 사용
- Annotation에 `groups = {}` 지정

### 7.10. groups vs DTO 분리
실무에서는 DTO를 분리하는 방식 선호
- 그룹 방식은 복잡하고 유지보수 어려움
- 어떤 필드가 어떤 상황에서 검증되는지 한눈에 파악하기 어려움
- `SaveRequestDto`, `UpdateRequestDto`로 역할 분리

---

## 8. SOLID 원칙
1. **SRP(단일 책임 원칙)**: 하나의 클래스는 하나의 책임만 가져야 한다.
2. **OCP(개방 폐쇄 원칙)**: 확장에는 열려 있고, 수정에는 닫혀 있어야 한다.
3. **LSP(리스코프 치환 원칙)**: 자식 클래스는 부모 클래스를 대체할 수 있어야 한다.
4. **ISP(인터페이스 분리 원칙)**: 작은 인터페이스 여러 개가 큰 범용 인터페이스보다 낫다.
5. **DIP(의존관계 역전 원칙)**: 구체적인 클래스가 아닌 추상화(인터페이스)에 의존해야 한다.

---


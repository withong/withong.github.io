+++
title = "Spring 기초 2"
date = "2025-03-18T17:32:05+09:00"
draft = false
topic = ["records"]
tag = ["내일배움캠프", "Spring"]
+++

---

## 1. 프레임워크와 라이브러리
### 프레임워크 (Framework)
- 소프트웨어 개발을 간편하게 만들기 위한 개발 환경.
- 일정한 틀(Frame) 안에서 개발해야 하며, 개발의 일관성을 제공함.
- 장점: 코드의 일관성, 보안 강화, 테스트 환경 제공, 커뮤니티 지원.
- 단점: 학습 곡선이 높음, 새로운 버전과의 호환성 문제 발생 가능, 구조 변경 어려움.

### 라이브러리 (Library)
- 특정 기능을 수행하는 코드의 모음.
- 개발자가 필요할 때 가져다 사용 가능.
- 장점: 개발 생산성 증가, 안정성이 높은 코드 활용 가능.
- 단점: 업데이트 또는 지원 중단 시 문제 발생, 호환성 이슈.

---

## 2. Annotation
- 코드에 메타데이터를 추가하는 기능으로, 컴파일러나 런타임에서 특정 동작을 트리거함.
- 주요 어노테이션
  - `@Override`: 상위 클래스의 메서드를 오버라이드함.
  - `@Deprecated`: 더 이상 사용되지 않는 코드에 붙이며, 경고 발생.
  - `@SuppressWarnings`: 컴파일 경고 억제.

- 사용자 정의 어노테이션도 가능하며, AOP (Aspect-Oriented Programming) 등의 기술과 결합하여 활용됨.

---

## 3. Lombok
- 반복적인 코드(Boilerplate Code)를 자동으로 생성해주는 라이브러리.
- 주요 어노테이션
  - `@Getter, @Setter`: getter/setter 자동 생성.
  - `@ToString`: `toString()` 자동 생성.
  - `@EqualsAndHashCode`: `equals()` 및 `hashCode()` 자동 생성.
  - `@NoArgsConstructor, @AllArgsConstructor`: 생성자 자동 생성.
  - `@Data`: `@Getter, @Setter, @ToString` 등을 포함한 종합 어노테이션.
  - `@Builder`: 빌더 패턴 적용.
  - `@Slf4j`: 로깅 객체 자동 생성.

---

## 4. Spring Framework와 Spring Boot
### Spring Framework
- Java 기반의 엔터프라이즈 애플리케이션 개발 프레임워크.
- 대형 시스템(쇼핑몰, 금융 등) 개발 시 많은 트래픽과 보안, 유지보수 문제를 해결해 줌.
- 특징
  - 객체지향 프로그래밍(OOP)의 특징(캡슐화, 상속, 다형성 등) 활용.
  - 유연한 애플리케이션 구성 가능.
  - 오픈소스 및 다양한 모듈 지원.

### Spring Boot
- Spring Framework를 더 쉽고 빠르게 사용할 수 있도록 만든 도구.
- Spring Boot의 특징
  - 자동 설정(Auto-configuration): 설정 없이 기본값으로 동작.
  - 내장 웹 서버(Tomcat 포함): 별도의 WAS 설치 없이 실행 가능.
  - 스타터 패키지 제공: `spring-boot-starter-web` 추가 시 자동으로 웹 애플리케이션 환경 구성.

---

## 5. Gradle (빌드 관리 도구)
- Groovy 기반의 빌드 자동화 도구로, 다양한 소프트웨어 빌드를 유연하게 처리할 수 있음.
- 특징
  - Build Cache: 빌드 결과물 재사용하여 속도 향상.
  - 점진적 빌드: 변경된 부분만 빌드하여 효율성 증가.
  - 멀티 프로젝트 빌드 지원: 독립적인 모듈을 관리 가능.
  - 설정 주입 방식: 프로젝트별 설정을 다르게 적용 가능.

### build.gradle
- 프로젝트의 빌드 설정을 관리하는 파일.
- 주요 구성 요소
  - 플러그인
    ```groovy
    plugins {
        id 'java'
        id 'org.springframework.boot' version '3.1.5'
        id 'io.spring.dependency-management' version '1.1.3'
    }
    ```
  - 의존성 관리
    ```groovy
    dependencies {
        implementation 'org.springframework.boot:spring-boot-starter-web'
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
        compileOnly 'org.projectlombok:lombok'
        runtimeOnly 'com.mysql:mysql-connector-j'
    }
    ```
  - 저장소 설정
    ```groovy
    repositories {
        mavenCentral()
    }
    ```

---

## 6. Java 웹 기술의 역사
### ① Servlet의 등장 (1997)
- Java 기반의 웹 개발이 시작됨.
- 클라이언트 요청을 받아 동적으로 HTML을 생성.
- 단점: 코드가 복잡하고 유지보수 어려움.

### ② JSP (JavaServer Pages) 도입 (1999)
- HTML 코드 내에 Java 코드를 삽입할 수 있도록 개선됨.
- 단점: 비즈니스 로직과 UI 분리가 어려움.

### ③ Servlet & JSP 기반의 MVC 패턴 도입
- MVC (Model-View-Controller) 패턴 적용하여 구조화된 개발 방식 등장.
- 장점: 유지보수성 향상, 역할 분리 가능.
- 단점: 반복적인 설정 작업이 필요함.

### ④ MVC 프레임워크의 등장 (2000~2010)
- Struts, Spring MVC 등의 프레임워크 등장.
- 장점: 설정 자동화 및 구조적인 개발 가능.
- 단점: 여전히 복잡한 설정이 존재.

### ⑤ Annotation 기반의 Spring MVC (2007~현재)
- 어노테이션(`@Controller`, `@RequestMapping` 등) 활용하여 설정 복잡성 감소.
- 장점: 간결하고 직관적인 웹 개발 가능.

### ⑥ Spring Boot 등장 (2014~현재)
- 기존 Spring Framework의 설정을 자동화하고 내장 웹 서버 제공.
- 장점: 빠른 애플리케이션 개발 가능.

---

## 7. 최신 Java 웹 기술 동향
### ① Spring MVC (Web Servlet)
- 동기 프로그래밍 기반으로 안정적인 웹 애플리케이션 개발 가능.
- 현재 가장 많이 사용되는 방식.

### ② Spring WebFlux (Web Reactive)
- 비동기 & 논블로킹(Non-blocking) 방식을 지원하는 웹 프레임워크.
- 높은 동시성을 요구하는 시스템에서 사용되며, Netty 등의 비동기 서버 기반으로 동작.

---
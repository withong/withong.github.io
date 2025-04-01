+++
title = "Spring 기초 3"
date = "2025-03-18T18:00:16+09:00"
draft = false
topic = ["record"]
tag = ["내일배움캠프", "Spring"]
+++

---

## 1. MVC 패턴
### Template Engine (템플릿 엔진)
- 동적인 웹 페이지를 생성하는 도구
- HTML과 데이터를 결합해 서버에서 페이지를 렌더링함
- 대표적인 템플릿 엔진
  - Thymeleaf → Spring과 통합이 잘 되어 가장 많이 사용됨
  - JSP (Java Server Pages) → 예전에는 많이 사용했으나 현재는 거의 사용되지 않음
  - FreeMarker, Velocity, Mustache → 특정 환경에서 사용됨

과거에는 Java 코드로 직접 HTML을 생성했으나, 유지보수가 어려워서 HTML에 필요한 부분만 Java 코드로 처리하도록 발전함

---

### MVC 패턴 개요
- 원래 Servlet과 JSP만으로 웹 개발을 했으나, 비즈니스 로직과 UI가 한 파일에 섞이면서 유지보수가 어려워짐
- MVC (Model-View-Controller) 패턴은 역할을 분리하여 코드의 유지보수를 쉽게 하기 위해 등장함

---

### MVC 패턴 구조
| 컴포넌트 | 설명 |
|-------------|---------|
| Model | 데이터를 저장하고 처리하는 역할 (비즈니스 로직) |
| View | 사용자에게 보여주는 UI (HTML, 템플릿 엔진) |
| Controller | 사용자의 요청을 받아 Model과 View를 연결 |

- View가 변경되어도 Model(비즈니스 로직)에 영향을 주지 않도록 분리  
- Model이 변경되어도 View를 수정할 필요가 없음

---

### MVC 패턴의 문제점
- 중복 코드 발생  
  - `dispatcher.forward(request, response)` 호출이 중복됨  
  - View의 경로 (`/WEB-INF/views/...jsp`)가 코드 곳곳에 하드코딩되어 변경이 어렵다
- 공통 기능이 많아질수록 Controller의 역할이 커짐
  - 로깅, 인증, 인가 등의 기능이 모든 Controller에 중복 작성됨

→ 프론트 컨트롤러 패턴

---

## 2. 프론트 컨트롤러 패턴
### 프론트 컨트롤러란?
- 모든 요청을 하나의 컨트롤러(Servlet)에서 처리하는 패턴
- 공통 기능 (로그인 체크, 보안, 예외 처리 등)을 한 곳에서 처리할 수 있음

프론트 컨트롤러의 역할  
1. 모든 요청을 받음 (`DispatcherServlet`이 역할 수행)  
2. 공통 기능을 처리 (인증, 보안, 로깅 등)  
3. 해당 요청을 적절한 Controller로 전달  

프론트 컨트롤러의 문제점  
- 모든 Controller가 동일한 방식으로 응답해야 함 → 확장성이 떨어짐

→ 어댑터 패턴 도입

---

## 3. 어댑터 패턴
### 어댑터 패턴이란?
- 각기 다른 인터페이스를 가진 객체들이 함께 동작할 수 있도록 연결하는 패턴
- Spring에서는 Handler Adapter가 역할을 수행함

장점  
- 새로운 컨트롤러가 추가되더라도 기존 공통 로직을 변경할 필요 없음  
- 컨트롤러마다 다른 응답 형식을 사용할 수 있음  

---

## 4. Spring MVC
- MVC 패턴 + 프론트 컨트롤러 패턴 + 어댑터 패턴이 적용된 웹 프레임워크

---

### Spring MVC 구조
1. 클라이언트 요청 → `DispatcherServlet`이 요청을 받음
2. Handler Mapping → 해당 요청을 처리할 `Controller`를 찾음
3. Handler Adapter → `Controller`가 다르면 적절한 어댑터를 사용하여 호출
4. Controller 실행 → 요청 처리 후 ModelAndView 반환
5. ViewResolver → 어떤 View를 렌더링할지 결정
6. View 렌더링 → 사용자에게 응답

---

### DispatcherServlet
- Spring의 프론트 컨트롤러 역할
- 요청을 받아서 적절한 `HandlerMapping`, `HandlerAdapter`, `ViewResolver`를 실행

→ `DispatcherServlet`이 모든 요청을 받아서 Spring 내부의 여러 기능을 조합하여 처리함

---

### Handler Mapping & Handler Adapter
| 컴포넌트 | 설명 |
|-------------|---------|
| Handler Mapping | 요청 URL과 매칭되는 컨트롤러를 찾음 |
| Handler Adapter | 해당 컨트롤러를 실행할 수 있도록 도와줌 |

Spring Boot에서는 자동으로 Handler Mapping과 Handler Adapter가 등록됨

---

### Spring Boot의 자동 구성 (Handler Mapping & Adapter)
| HandlerMapping 종류 | 설명 |
|-------------------|---------|
| RequestMappingHandlerMapping | `@RequestMapping`이 붙은 컨트롤러를 매핑 |
| BeanNameUrlHandlerMapping | `Bean 이름`으로 매핑 |

<br>

| HandlerAdapter 종류 | 설명 |
|-------------------|---------|
| RequestMappingHandlerAdapter | `@RequestMapping` 컨트롤러 실행 |
| HttpRequestHandlerAdapter | `HttpRequestHandler` 인터페이스 실행 |
| SimpleControllerHandlerAdapter | 옛날 `Controller` 인터페이스 실행 |

---

## 5. View Resolver
### View Resolver란?
- 컨트롤러에서 반환한 View 이름을 실제 View(JSP, Thymeleaf 등)로 변환하는 역할

ViewResolver 동작 방식  
1. Controller가 `ModelAndView` 객체를 반환  
   → `model.addAttribute("message", "Hello Spring")`  
   → `return "home";`  
2. View Resolver가 View 이름을 실제 경로로 변환  
   → `/WEB-INF/views/home.jsp`  
3. View 렌더링 및 응답 반환  

---

### Spring Boot에서 자동 등록되는 ViewResolver
| ViewResolver 종류 | 설명 |
|----------------------|---------|
| BeanNameViewResolver | Bean 이름을 기반으로 View를 찾음 |
| InternalResourceViewResolver | `prefix`와 `suffix`를 조합하여 View 경로 결정 |

Spring Boot에서는 `application.properties`에서 ViewResolver 설정 가능

```properties
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

---

## 6. Spring MVC 동작 순서 정리
1. 클라이언트가 HTTP 요청을 보냄
2. DispatcherServlet이 요청을 받음
3. Handler Mapping이 요청을 처리할 컨트롤러를 찾음
4. Handler Adapter가 적절한 컨트롤러를 실행함
5. Controller가 `ModelAndView`를 반환함
6. View Resolver가 적절한 View를 찾아 렌더링함
7. 최종적으로 클라이언트에게 응답이 반환됨

---

## 7. InternalResourceViewResolver 동작 방식
- `InternalResourceViewResolver`는 application.properties 설정을 기반으로 View 경로를 변환
- `return "home";` → `/WEB-INF/views/home.jsp`로 변환 후 View를 렌더링

Thymeleaf는 추가 설정 없이 자동으로 View를 렌더링함

---
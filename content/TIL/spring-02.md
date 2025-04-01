+++
title = "Querydsl"
date = "2025-03-31T22:21:19+09:00"
draft = true
topic = ["TIL"]
tag = ["Study", "Spring", "JPA", "Querydsl"]
+++

---

### 1단계. Querydsl이 뭔지부터 이해하기

**✅ Querydsl이란?**  
Java 코드로 SQL 쿼리를 타입 안정성 있게 작성할 수 있도록 도와주는 도구예요.  
예를 들어, 문자열로 SQL을 짜는 대신 객체와 메서드 체이닝으로 `where`, `select` 등을 조립해요.

**✅ 왜 쓰는 걸까?**
- 동적 쿼리(조건이 있을 수도 있고 없을 수도 있는 쿼리) 작성이 깔끔함
- 컴파일 시점에 오류를 잡아줌 (SQL은 런타임 오류)
- 유지보수성 높음

---

### 2단계. Querydsl 설정부터 시작하자

이제 프로젝트에 **Querydsl을 적용**할 수 있도록 설정을 도와드릴게요.  
지금 사용하는 **빌드 도구가 Gradle**이라면, 아래 순서로 하면 돼요.

---

### ✅ 2-1. build.gradle 설정 추가

`build.gradle`에 다음 내용을 추가해줘요.

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.1.2'
    id 'io.spring.dependency-management' version '1.1.0'
    id 'com.ewerk.gradle.plugins.querydsl' version '1.0.10' // 추가
}

dependencies {
    implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
    annotationProcessor 'com.querydsl:querydsl-apt:5.0.0:jakarta'
    annotationProcessor 'jakarta.persistence:jakarta.persistence-api'
    annotationProcessor 'jakarta.annotation:jakarta.annotation-api'
}

def querydslDir = "src/main/generated"

querydsl {
    jpa = true
    querydslSourcesDir = querydslDir
}

sourceSets {
    main.java.srcDirs += [querydslDir]
}

tasks.withType(JavaCompile) {
    options.annotationProcessorGeneratedSourcesDirectory = file(querydslDir)
}
```

> 위 내용에서 프로젝트에 맞는 Spring Boot 버전만 조심해서 맞춰주면 돼요.  
> 현재 `jakarta` 기반을 사용하고 있어서 위 버전이 맞습니다.

---

### ✅ 2-2. Q타입 클래스 생성

Gradle 설정 후, **터미널(PowerShell)**에서 아래 명령어 실행:

```powershell
./gradlew clean build
```

이걸 실행하면, `src/main/generated` 폴더에 `QSchedules`, `QUsers` 같은 클래스가 자동으로 생성돼요.  
이게 바로 Querydsl에서 사용하는 **Q타입 클래스**예요.

---



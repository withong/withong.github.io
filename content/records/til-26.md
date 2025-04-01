+++
title = "Bcrypt를 활용한 비밀번호 암호화"
date = "2025-04-01T16:50:00+09:00"
draft = true
topic = ["records"]
tag = ["내일배움캠프", "Java", "과제", "일정 관리 API", "암호화"]
+++

<hr>
<br>

## ✅ bcrypt란?

> **bcrypt는 비밀번호를 안전하게 암호화(해시)하기 위한 알고리즘이에요.**  

단순한 암호화가 아니라, **특수한 목적**을 가진 **비밀번호 전용 해시 함수**예요.  
1999년에 처음 제안되었고, 지금도 많은 서비스(Google, Facebook 등)에서 사용돼요.  

bcrypt는 전 세계의 대부분의 언어에서 사용할 수 있는 표준 암호화 알고리즘

---

## ✅ bcrypt의 역할

> "사용자의 비밀번호를 **복호화 불가능한 형태**로 안전하게 저장하기"

---

## 🔐 왜 비밀번호는 복호화되면 안 될까?

- 누군가 DB를 털어도 **비밀번호를 알 수 없어야 해요.**
- `암호화 → 복호화`가 가능한 알고리즘은 위험해요.
- 그래서 **"해시(hash)" 알고리즘**을 써요.
  - 해시는 **단방향(one-way)**: 다시 원래 값으로 되돌릴 수 없음.

---

## 💥 하지만 일반 해시는 위험해요

### 예: SHA-256으로 비밀번호 `1234` 해시

```plaintext
1234 → a5bfc9e07964f8dddeb95fc584cd965d
```

이 값은 **항상 똑같아요.**  
그래서 **무차별 대입 공격(Brute Force), 무작위 대입 공격(Rainbow Table)**에 취약해요.

---

## ✅ bcrypt의 특징 (보안성이 높은 이유)

| 특징 | 설명 |
|------|------|
| 🌱 Salt 자동 적용 | 동일한 비밀번호라도 해시 결과가 **매번 다르게 생성됨** |
| 🐢 느린 계산 속도 | 일부러 느리게 처리해서, 무차별 공격에 **시간 많이 걸리게 함** |
| 🔁 반복 횟수 조정 가능 | `cost` 값을 높여서 해시 처리 복잡도 조절 가능 |
| 🔐 단방향 해시 | 복호화가 불가능함. **일치 여부는 비교로만 판단**

---

## 📌 예시

비밀번호 `hello123!`을 bcrypt로 해시하면:

```plaintext
$2a$10$gDs0k2eVqHqE2sVvzmHxuOdqOcL1K/hbBAyMe9SptNUoCJ9LD7f9W
```

- `$2a$10$` → 알고리즘 버전 + cost 값 (10번 반복)
- 뒷부분 → salt와 해시값이 결합된 형태

> 똑같은 `hello123!`이어도, 매번 다른 해시값이 생성돼요.

---

## 🧪 비교는 어떻게?

```java
BCrypt.verifyer().verify("hello123!".toCharArray(), 암호화된문자열);
```

- 입력한 비밀번호를 해시한 뒤,
- 기존 암호화된 값과 내부적으로 비교
- 둘이 일치하면 `true`, 아니면 `false`

---

## ✅ 정리

| 항목 | 내용 |
|------|------|
| bcrypt란 | 비밀번호를 **복호화할 수 없게** 만드는 해시 함수 |
| 왜 필요해? | DB 탈취 시에도 비밀번호 노출을 막기 위해 |
| 특징 | 매번 다른 결과, 느린 계산 속도, 비교 가능하지만 복호화 불가 |
| 우리가 할 일 | 사용자가 입력한 비밀번호를 bcrypt로 `암호화 → 저장`, 로그인 시 `비교만` 수행

---

## 해시란?
> **어떤 데이터를 고정된 길이의 "이상한 문자열"로 바꿔주는 것.**  
> 그리고 이 과정은 **되돌릴 수 없어요.**

---

### 🔐 예시

```plaintext
입력값: hello123!
↓ 해시함수
결과: a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
```

이때 나온 긴 문자열이 **해시값(hash value)**이고,  
입력값과는 완전히 다르게 생겼죠?

---

## 🔁 복호화는 될까?

**절대 안 돼요.**

- 해시함수는 **단방향(one-way)** 함수예요.
- 한 번 해시하면 **원래 값으로 되돌릴 수 없어요.**
- 그래서 해시는 **"암호화"와는 조금 달라요.**
  - 암호화는 보통 `복호화`가 가능한 걸 말함
  - 해시는 복호화 불가. 비교만 가능

---

## 🧩 해시의 특징

| 특징 | 설명 |
|------|------|
| ✅ 고정된 길이 | 입력이 1글자든 1000글자든, 결과는 항상 같은 길이 |
| ✅ 빠름 | 해시 연산은 속도가 빠름 (bcrypt 같은 일부 예외 제외) |
| ✅ 단방향 | 되돌릴 수 없음. 복호화 불가 |
| ✅ 같은 입력 → 같은 출력 | 같은 문자열이면 항상 같은 해시값 |
| ✅ 입력 조금만 달라도 → 완전히 다른 결과 | 보안상 좋은 특징

---

## 🛡️ 그래서 해시는 어디에 쓰일까?

1. **비밀번호 저장**
   - 평문 저장 ❌
   - 해시값 저장 ✅  
     → 로그인 시에도 해시해서 **비교만** 함

2. **데이터 무결성 확인**
   - 파일이 변조되었는지 확인할 때 사용  
     → 파일 해시값을 비교

3. **디지털 서명 / 인증서**
   - 암호화 시스템에서 핵심 역할

4. **블록체인**
   - 블록마다 해시가 연결되어 있음

---

## ✅ 대표적인 해시 알고리즘

| 이름 | 특징 |
|------|------|
| MD5 | 예전에는 많이 썼지만 보안 취약 |
| SHA-1 | 이젠 거의 안 씀 (충돌 공격에 취약) |
| SHA-256 | 안전하고 널리 쓰임 |
| bcrypt | 비밀번호용. 일부러 느리게 만든 해시 |
| PBKDF2, Argon2 | bcrypt와 유사한 비밀번호 해시용 알고리즘

---

## 🔍 해시 vs 암호화 차이

| 항목 | 해시 | 암호화 |
|------|------|--------|
| 되돌릴 수 있음? | ❌ 복호화 불가 | ✅ 복호화 가능 |
| 목적 | 비교, 무결성 확인 | 비밀 전달 |
| 비밀번호 저장 | ✅ | ❌ 위험 |
| 알고리즘 예시 | SHA-256, bcrypt | AES, RSA |

---

## ✅ 마무리 요약

> **해시란?**
> 입력값을 고정된 길이의 복잡한 문자열로 바꾸는 과정.  
> 복호화 불가능, 되돌릴 수 없음.  
> **비교**만 가능해서 비밀번호 저장에 딱 좋음.

---

**"비밀번호를 해시했더니 매번 다른 값이 나오는데,  
그럼 로그인할 때 도대체 어떻게 비교해서 맞는지 알 수 있지?"**

> bcrypt는 **매번 다른 결과를 만들지만**,  
> 그 안에 **비교에 필요한 정보(salt + cost + 해시값)**가 **전부 들어 있어서**,  
> 나중에 `비교만` 하면 **일치 여부를 정확하게 판단할 수 있어요.**

---

## 🔐 예를 들어 볼게요

비밀번호: `hello123!`

이걸 bcrypt로 해싱하면 예를 들어 이런 값이 나올 수 있어요:

```plaintext
$2a$10$gDs0k2eVqHqE2sVvzmHxuOdqOcL1K/hbBAyMe9SptNUoCJ9LD7f9W
```

이 문자열 안에는 다음 정보가 **모두 포함돼 있어요**:

| 구성 | 설명 |
|------|------|
| `$2a$` | bcrypt 알고리즘 버전 |
| `10$` | cost factor (반복 횟수) |
| `gDs0k2eVqHqE2sVvzmHxuO` | salt (랜덤 값) |
| `dqOcL1K/hbBAyMe9SptNUoCJ9LD7f9W` | 해시된 비밀번호 결과 |

---

## ✅ 로그인할 때 무슨 일이 벌어지냐면

1. 사용자가 평문 비밀번호 입력: `"hello123!"`
2. 저장된 bcrypt 해시값 전체를 불러옴
3. **그 해시값 안에서 salt, cost 등을 추출**
4. **입력한 비밀번호 + 추출한 salt로 다시 해시**
5. 해시 결과가 저장된 값과 **일치하는지 비교**

→ bcrypt가 이 과정을 **내부적으로 자동으로** 해줘요.

---

## 🔍 그래서 `bcrypt.verifyer().verify(...)`는 이렇게 작동해요

```java
BCrypt.verifyer().verify("hello123!".toCharArray(), 해시된문자열);
```

- 여기서 두 번째 파라미터인 "해시된문자열" 안에는
  - salt도 들어있고
  - cost도 들어있고
  - 알고리즘 버전도 들어 있어서
- bcrypt는 이걸 자동으로 분석해서 **비교할 수 있게 해줘요**

---

## ✅ 마무리 정리

| 질문 | 답변 |
|------|------|
| 해시 결과가 매번 다르면 비교는 어떻게 해? | bcrypt 해시 안에 salt와 비교 정보가 다 들어 있어서, 다시 계산해서 비교함 |
| salt는 어디서 오는데? | bcrypt가 자동으로 생성하고, 해시값 안에 포함시켜줌 |
| 복호화 없이 비교가 가능하다고? | 네, 해시는 단방향이지만 "비교는 가능"해요 (같은 비밀번호 + 같은 salt → 같은 결과)

---

프로젝트 적용

## ✅ 전체 코드

```java
package task.schedule.config;

import at.favre.lib.crypto.bcrypt.BCrypt;
import org.springframework.stereotype.Component;

@Component
public class PasswordEncoder {

    public String encode(String rawPassword) {
        return BCrypt.withDefaults().hashToString(BCrypt.MIN_COST, rawPassword.toCharArray());
    }

    public boolean matches(String rawPassword, String encodedPassword) {
        BCrypt.Result result = BCrypt.verifyer().verify(rawPassword.toCharArray(), encodedPassword);
        return result.verified;
    }
}
```

---

## ✅ 한 줄씩 자세히 설명

### 🔹 `package task.schedule.config;`

- 이 클래스가 어떤 패키지에 속해 있는지 나타내는 선언이에요.
- 프로젝트 구조상 `config` 패키지에 넣는 게 맞고, 나중에 `SecurityConfig` 같은 설정 클래스들과 함께 두면 관리하기 좋아요.

---

### 🔹 `import at.favre.lib.crypto.bcrypt.BCrypt;`

- 우리가 `build.gradle`에 추가한 **bcrypt 라이브러리의 핵심 클래스**예요.
- 여기서 `BCrypt.withDefaults()`, `BCrypt.verifyer()` 같은 메서드를 사용할 수 있게 해줘요.

---

### 🔹 `import org.springframework.stereotype.Component;`

- 이건 스프링의 어노테이션이에요.
- `@Component`가 붙은 클래스는 **스프링이 자동으로 객체(bean)로 등록해줘요.**
- 덕분에 다른 클래스에서 `@Autowired` 또는 `@RequiredArgsConstructor`로 주입 받을 수 있어요.

---

### 🔹 `@Component`

- 위에서 설명한 대로, 이 클래스를 **스프링 컨테이너에 등록**시키는 역할이에요.
- 등록된 덕분에, 다른 클래스에서 `private final PasswordEncoder passwordEncoder;`로 사용 가능해요.

---

### 🔹 `public class PasswordEncoder`

- 클래스 선언이에요.
- 이름은 Spring Security의 `PasswordEncoder`랑 이름이 같지만, **우리가 직접 구현한 사용자 정의 클래스**예요.

---

## ✅ 첫 번째 메서드

```java
public String encode(String rawPassword) {
    return BCrypt.withDefaults().hashToString(BCrypt.MIN_COST, rawPassword.toCharArray());
}
```

### 🔸 `public String encode(String rawPassword)`

- 이 메서드는 **비밀번호를 암호화해서 문자열로 반환**해요.
- `rawPassword`: 사용자가 입력한 평문 비밀번호 (예: `hello123!`)

### 🔸 `BCrypt.withDefaults()`

- bcrypt 해시 함수를 기본 설정으로 사용한다는 의미예요.
- 기본값은 안전하게 구성되어 있어요.

### 🔸 `.hashToString(...)`

- 입력한 비밀번호를 **해시(=암호화)** 해서 문자열로 변환해요.

### 🔸 `BCrypt.MIN_COST`

- bcrypt는 내부적으로 연산을 몇 번 반복할지를 `cost` 값으로 지정해요.
- `MIN_COST`는 가장 낮은 수준(4)이지만, **연습이나 테스트 환경에서 빠르게 처리할 때** 적절해요.
- 실제 운영환경에서는 `12` 정도로 올리는 게 일반적이에요.

### 🔸 `rawPassword.toCharArray()`

- 비밀번호를 `char[]`로 바꿔서 넘기는 이유는,
- **문자열(String)은 불변(immutable)**이라 메모리에 오래 남아 있을 수 있어요.
- 반면 `char[]`는 내용을 지우는 게 가능해서 **보안에 더 안전**해요.

---

## ✅ 두 번째 메서드

```java
public boolean matches(String rawPassword, String encodedPassword) {
    BCrypt.Result result = BCrypt.verifyer().verify(rawPassword.toCharArray(), encodedPassword);
    return result.verified;
}
```

### 🔸 `public boolean matches(String rawPassword, String encodedPassword)`

- 이 메서드는 **입력한 평문 비밀번호와 저장된 암호화된 비밀번호가 같은지 확인**해줘요.
- 로그인할 때 사용하는 메서드예요.

### 🔸 `BCrypt.verifyer().verify(...)`

- bcrypt에서 제공하는 **비교 기능**이에요.
- 내부적으로 encodedPassword 안의 **salt, cost** 정보를 이용해서  
  rawPassword를 다시 해시한 후, 둘이 같은지 비교해요.

### 🔸 `result.verified`

- 비교 결과가 성공이면 `true`, 아니면 `false`를 반환해요.

---

## ✅ 마무리 요약

| 메서드 | 역할 |
|--------|------|
| `encode()` | 비밀번호를 해시(암호화)해서 DB에 저장할 문자열로 변환 |
| `matches()` | 입력한 비밀번호가 저장된 해시와 일치하는지 확인 |



<br>
<hr>
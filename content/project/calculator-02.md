+++
title = "계산기 Lv 2"
date = "2025-02-28T21:00:00+09:00"
draft = false
tag = ["내일배움캠프", "과제"]
+++

## 요구사항
클래스를 적용해 기본적인 연산을 수행할 수 있는 계산기 만들기

- 사칙연산을 수행 후, 결과값 반환 메서드 구현 & 연산 결과를 저장하는 컬렉션 타입 필드를 가진 Calculator 클래스를 생성
    - 사칙연산을 수행한 후, 결과값을 반환하는 메서드 구현
    - 연산 결과를 저장하는 컬렉션 타입 필드를 가진 Calculator 클래스를 생성
    - 1) 양의 정수 2개(0 포함)와 연산 기호를 매개변수로 받아 사칙연산(➕,➖,✖️,➗) 기능을 수행한 후 2) 결과 값을 반환하는 메서드와 연산 결과를 저장하는 컬렉션 타입 필드를 가진 Calculator 클래스를 생성합니다.

- **Lv 1에서 구현한 App 클래스의 main 메서드에 Calculator 클래스가 활용될 수 있도록 수정**
    - 연산 수행 역할은 Calculator 클래스가 담당
        - 연산 결과는 Calculator 클래스의 연산 결과를 저장하는 필드에 저장
    - 소스 코드 수정 후에도 수정 전의 기능들이 반드시 똑같이 동작해야합니다.

- **App 클래스의 main 메서드에서 Calculator 클래스의 연산 결과를 저장하고 있는 컬렉션 필드에 직접 접근하지 못하도록 수정 (캡슐화)**
    - 간접 접근을 통해 필드에 접근하여 **가져올** 수 있도록 구현합니다. (Getter 메서드)
    - 간접 접근을 통해 필드에 접근하여 **수정할** 수 있도록 구현합니다. (Setter 메서드)
    - 위 요구사항을 모두 구현 했다면 App 클래스의 main 메서드에서 위에서 구현한 메서드를 활용 해봅니다.

- Calculator 클래스에 저장된 연산 결과들 중 가장 먼저 저장된 데이터를 삭제하는 기능을 가진 메서드를 구현한 후 App 클래스의 main 메서드에 삭제 메서드가 활용될 수 있도록 수정
    - 키워드 : `컬렉션`
        - 컬렉션에서 ‘값을 넣고 제거하는 방법을 이해한다.’가 중요합니다!


---

## 기능 구현

<br>

### CalculationRecord
계산 기록을 저장하는 역할을 하는 클래스  

![](https://velog.velcdn.com/images/ezro/post/e0d3ec58-d925-4509-9ea1-bbf446978c07/image.png)

- 각 연산의 입력값, 연산자, 결과값을 하나의 객체로 관리
- 불변 객체로 설계되어 한 번 생성된 후 값 변경 불가
- `toString()` 오버라이딩을 통해 깔끔한 출력 형식 제공

<br>

### Calculator
계산기 기능을 제공하는 클래스

![](https://velog.velcdn.com/images/ezro/post/5522b7ea-ff44-456b-a4da-d478f6316672/image.png)

-  **`records`**  
   - 연산 결과를 저장할 `records` 리스트를 선언.
   - `private`으로 선언하여 외부에서 직접 접근 불가능 → 캡슐화 강화.

-  **`Calculator()`**  
   - 클래스가 생성될 때 `records` 리스트 초기화

-  **`setUserNumber()`**  
  숫자를 입력받는 메서드.  
  사용자로부터 입력받은 숫자가 올바른 값인지 검증 후 반환.  
   - 사용자의 입력을 `trim()`하여 공백 제거.
   - `nextLine()`을 사용하여 엔터 입력도 인식할 수 있도록 함.
   - 아무 값도 입력하지 않으면 오류 메시지를 출력하고 다시 입력 요청.
   - 정규식 `\\d+`를 사용하여 0 이상의 정수만 입력 가능하도록 검증.  
     문자, 소수점 숫자 등은 허용되지 않음.
   - `int`로 변환할 수 없는 값이 입력되면 오류 출력 후 다시 입력 요청.

![](https://velog.velcdn.com/images/ezro/post/7bb5088b-4ed1-489e-aa9b-8940dfbc1bba/image.png)

-  **`setUserOperator()`**  
  사용자로부터 연산자를 입력받아 검증 후 반환하는 메서드.
     - 연산자로 사용할 수 있는 값들을 문자열 `operators`에 저장.
     - 입력값이 한 글자인지 확인 후, 연산자로 사용 가능한 값인지 검증.
     - 정상적인 연산자(`+`, `-`, `*`, `/`)가 입력되면 반환.
     - 잘못된 입력이면 오류 메시지를 출력하고 다시 입력 요청.

![](https://velog.velcdn.com/images/ezro/post/30150e87-8fd9-4b12-9acf-c35d5f312ba0/image.png)
 
-  **`calculate()`**  
    입력받은 두 숫자와 연산자를 사용하여 계산을 수행하는 메서드.
    - `Math.~Exact()`를 사용하여 `int` 범위 초과 감지.
    - 0으로 나누려 할 경우 오류 출력 후 `정의되지 않음` 반환.
    - 잘못된 연산자가 입력되었을 경우 오류 출력 후 `Error` 반환.
    - 연산 중 `int` 범위를 초과하면 오류 출력 후 `연산 범위 초과` 반환.
    - 정상적인 경우 `String`으로 변환하여 연산 결과 반환.

![](https://velog.velcdn.com/images/ezro/post/0dafc24a-88f4-4482-9c00-7f3855dc97c8/image.png)

-  **`handleError()`**  
  오류 발생 시 사용자에게 적절한 메시지를 출력하는 메서드.

![](https://velog.velcdn.com/images/ezro/post/4cd38f07-9f01-4da8-91d8-2f41567a38e6/image.png)

-  **`updateRecords()`**  
  새로운 연산 결과를 `records`에 추가하는 메서드.  
   - 정규식을 활용하여 숫자와 문자열(오류 메시지) 구분
   - `-?` → 음수(-)일 수도 있고, 양수일 수도 있음.
   - `\\d+` → 반드시 하나 이상의 숫자(0~9)가 포함됨.
   - `(\\.\\d+)?` → 소수점 이하가 있을 수도 있고, 없을 수도 있음.

   - 숫자가 아닌 경우(오류 메시지인 경우) 그대로 저장
   - 숫자인 경우 `DecimalFormat`을 사용하여 포맷팅 후 저장
     - 소수점 8자리까지 반올림하여 저장.
     - 소수점 이하가 0이면 정수 형태로 변환.
  
   - 연산 결과를 `CalculationRecord` 객체로 변환 후 리스트에 추가
   - `trimRecords()` 호출하여 최대 10개까지만 저장되도록 유지

![](https://velog.velcdn.com/images/ezro/post/6f1ba043-44bd-49b3-9a47-28f873f4c5a7/image.png)

-  **`trimRecords()`**  
  기록 개수를 제한하는 메서드.
     - 최대 10개까지만 저장되도록 유지, 초과 시 가장 오래된 기록 삭제.

![](https://velog.velcdn.com/images/ezro/post/9fd97011-49ef-4d07-9a85-644bf90fde1a/image.png)

-  **`showRecords()`**  
    최근 기록 목록을 출력하는 메서드. (최대 10개)
    - 기록이 없으면 오류 메시지 출력 후 종료.
    - 오래된 순으로 번호를 부여하여 저장된 기록을 출력.

![](https://velog.velcdn.com/images/ezro/post/c43d3afa-a7e8-494f-9a26-62d37a0ff968/image.png)

-  **`showLastRecord()`**  
    최근 기록을 출력하는 메서드 (마지막 연산)  
    - 기록이 없으면 오류 메시지 출력 후 종료.
    - 마지막에 저장된 연산 기록을 출력.

![](https://velog.velcdn.com/images/ezro/post/566f9c5f-aa19-452a-9571-32ab36ce5547/image.png)

![](https://velog.velcdn.com/images/ezro/post/dbccb34a-930b-4261-bba3-bc787241e1bf/image.png)

![](https://velog.velcdn.com/images/ezro/post/de9fbe38-8992-44ae-82b1-1a0dde3b8601/image.png)

![](https://velog.velcdn.com/images/ezro/post/9db557b5-81c8-4ae9-ad2f-3982953d0f00/image.png)

-  **`removeRecords()`**  
    사용자로부터 입력을 받아 기록을 삭제하는 메서드.
    - 저장된 기록이 없는 경우, 메시지를 출력하고 메서드 종료.
      
    - 현재 저장된 기록을 `showRecords()`를 호출하여 화면에 출력.
    - 사용자가 선택할 수 있도록 기록을 보여준 후, 삭제할 번호를 입력받음.
    - 삭제할 기록 번호를 입력하라는 안내 메시지 출력.
      
    - `enter`를 눌러 빈 값을 입력하면 삭제를 취소하고 메서드를 종료.
    - `all`을 입력하면 전체 삭제 후 완료 메시지를 출력하고 메서드 종료.
      
    - 입력 값이 `enter`나 `all`이 아닌 경우, `int`로 변환 시도.
      
    - 숫자가 아닌 값이 입력되면 오류 메시지를 출력하고 재입력 요청.
    - 입력값이 범위를 벗어나면 오류 메시지를 출력하고 재입력 요청.
      
    - 입력된 번호가 존재하는 기록의 번호 범위안에 있는지 확인.
    - 입력값이 범위 내에 존재하면 리스트에서 해당 인덱스의 기록을 삭제.
    - 삭제 완료 후 완료 메시지를 출력하고 메서드 종료.

<br>

### App

![](https://velog.velcdn.com/images/ezro/post/11c5e5ae-67f8-4616-b5de-d926ec914bd5/image.png)

![](https://velog.velcdn.com/images/ezro/post/c95da45d-0d59-4488-98bc-ef8321087877/image.png)

![](https://velog.velcdn.com/images/ezro/post/f28652e3-73d6-4d3b-8bb3-2c903697af51/image.png)

-  **`main()`**  
  사용자 입력을 받아 계산기를 동작시키는 역할.  
  메뉴를 출력하고, `Calculator` 클래스를 사용하여 연산을 수행 및 기록 관리.  
   - 메뉴를 출력하고, 사용자 입력을 받아 선택한 메뉴의 번호를 저장.
   - `setUserChoice()` 메서드를 호출하여 사용자 입력 값 검증.
   - 사용자가 유효한 값을 입력할 때까지 반복 요청.

   - 사용자로부터 첫 번째 숫자, 연산자, 두 번째 숫자를 입력받음.
   - 각 입력은 `Calculator` 클래스의 메서드를 사용하여 검증 후 저장.
   - 입력받은 숫자와 연산자를 이용하여 `calculate()` 메서드로 연산 수행.
   - 결과값을 `String` 형식으로 반환받아 저장.
   - 연산 결과를 `updateRecords()`를 통해 `Calculator`의 기록 리스트에 저장.
   - `showLastRecord()`를 호출하여 가장 최근 계산 결과를 출력.

   - `showRecords()`를 호출하여 최근 10개의 계산 기록을 출력.
   - 기록이 없으면 "조회할 기록이 없습니다." 메시지 출력.

   - 사용자가 삭제할 기록을 선택할 수 있도록 `removeRecords()` 호출.
   - 삭제할 기록을 선택하거나 `all`을 입력하면 전체 삭제 가능.

   - 사용자가 4를 입력하면 종료 메시지를 출력 후 프로그램 종료.

![](https://velog.velcdn.com/images/ezro/post/a4f7c5c0-1a79-4bf4-bb8a-87d73dc61720/image.png)

-  **`setUserChoice()`**  
    사용자가 올바른 메뉴 번호를 입력할 때까지 반복 요청하는 메서드.
    - 메뉴 목록을 출력하고 사용자에게 입력을 요청.
    - 입력값이 1~4가 아니면 오류 메시지를 출력하고 다시 입력받음.
    - 정규식 `"^[1-4]$"`을 사용하여 한 자리 숫자(1~4)만 허용.
    - 정상적인 입력이라면 `int`로 변환 후 반환.

---

## 문제 및 해결

<br>

### 캡슐화 구현 방식
- **요구사항**
  - **App 클래스의 main 메서드에서 Calculator 클래스의 연산 결과를 저장하고 있는 컬렉션 필드에 직접 접근하지 못하도록 수정 (캡슐화)**
    - 간접 접근을 통해 필드에 접근하여 **가져올** 수 있도록 구현합니다. (Getter 메서드)
    - 간접 접근을 통해 필드에 접근하여 **수정할** 수 있도록 구현합니다. (Setter 메서드)
    - 위 요구사항을 모두 구현 했다면 App 클래스의 main 메서드에서 위에서 구현한 메서드를 활용 해봅니다.

- **문제**
  - 필수 기능 가이드에서는 getter/setter 메서드를 이용한 간접 접근을 요구하고 있지만, 내가 구현한 Calculator 클래스에서는 getter/setter 없이 필요한 기능을 직접 제공하는 메서드를 통해서만 기록을 조회/삭제/추가하도록 설계되어 있음.

  - records 리스트를 외부에서 직접 접근하지 못하도록 private으로 감추고, showRecords(), updateRecords(), removeRecords() 등의 메서드를 통해서만 조작이 가능함.

  - 가이드에서 명시한 getter/setter를 반드시 구현해야 하는지? 아니면, 현재 방식도 적절한 캡슐화로 인정될 수 있는지?

- **분석**
  - 현재 방식은 필드 값을 직접 노출하지 않으면서, 필요한 기능을 내부 메서드를 통해서만 수행하도록 제한함. 결과적으로 강력한 캡슐화가 이루어지며, 데이터를 직접 수정하는 것을 방지할 수 있음.

  - 하지만 **요구사항을 준수하지 않아도 괜찮은가?**

- **해결 및 개선**
  - 튜터님께 질문 → 평가는 요구사항을 기준으로 이루어지지만, 다른 방식으로 구현했다면 평가자들이 한눈에 알아볼 수 있게 주석 설명을 추가하면 좋을 듯함.

- **결과**
  - 주석 설명 추가
      ```
      이 'records' 필드는 private으로 감춰져 있습니다.
      별도의 getRecords(), setRecords() 기능을 제공하지 않고,
      showRecords(), showLastRecord(), removeRecords(), updateRecords() 메서드를 통해서만
      기록을 조회/삭제/추가하도록 구현하여 캡슐화를 강화했습니다.
      ```

---  
  
### 컬렉션 데이터 삭제 기능

- **요구사항**
  - Calculator 클래스에 저장된 연산 결과들 중 가장 먼저 저장된 데이터를 삭제하는 기능을 가진 메서드를 구현한 후 App 클래스의 main 메서드에 삭제 메서드가 활용될 수 있도록 수정

- **문제**
  - 필수 기능 가이드에서는 **가장 먼저 저장된 데이터를 삭제하는 메서드**를 구현한 후 **App 클래스의 main 메서드에서 활용**되도록 하라고 명시되어 있음. 하지만 나는 사용자가 원하는 값을 선택하여 삭제할 수 있도록 구현함. 

  _(또) 요구사항 불일치 이슈..._🙄  


- **분석**
  - 컬렉션을 활용하여 연산 결과를 저장하고 삭제하는 기능을 구현하는 목적은
  `1` 컬렉션 학습을 위한 요구사항인가?  
  `2` 추가 기능 개발 가능성을 고려한 요구사항인가?  

  - 가장 먼저 저장된 데이터를 삭제해야 하는 이유는  
  `1` 과제 수행에서 반드시 구현해야 하는 공통 기능인가?  
  `2` 아니라면 다른 방식으로 삭제 기능을 개발해도 되는가?  

- **해결 및 개선**
  - 튜터님께 질문 → 이 요구사항은 컬렉션 학습을 위한 것이므로 반드시 "가장 먼저 저장된 데이터"만 삭제할 필요는 없음. 컬렉션을 충분히 활용하여 삭제 기능을 구현했다면, 다른 방식으로 삭제를 개발해도 괜찮음.

- **결과**
  - 기존 코드 유지.

---

### 연산 오류 처리

- **문제**
  - 이전 버전에서는 연산 오류가 발생했을 때 -9999를 반환했었으나, 계산기 프로그램에서 오류 결과를 숫자로 처리하는 것이 부적절하다고 판단해서 NaN으로 결과값을 변경함. 하지만 NaN은 int 범위가 초과했을 때의 결과 값으로 부적절함.

- **분석**
  - 현재 연산 수행 결과로 나올 수 있는 오류 결과는
  `1` 0으로 나누기 시도 → NaN으로 처리  
  `2` int 범위 초과 → NaN 부적절함.  
  
  - 연산 수행 결과가 int 범위 초과시 double로 변환해서 반환한다면?  
  → 입력할 때는 int 범위 초과하면 안 되고 수행 결과는 int 범위 초과해도 되고? 프로그램의 일관성이 없고 논리적이지 않음.
  
  - int로 받던 입력값을 long으로 확장한다면?  
  → 다음 레벨에서 개선할 예정

- **해결 및 개선**
  - calculate()의 반환값을 String으로 변경하고 연산 수행 결과가 오류일 시 해당 오류에 맞는 결과를 문자열로 반환하도록 수정. 정상적인 연산은 연산 후 String.valueOf()을 사용하여 String으로 변환 후 반환.  
  
  - updateRecords()에서 문자열과 숫자를 구분하는 로직 추가. 문자열인 경우 오류 메시지를 그대로 출력하고, 숫자일 경우 포맷팅을 진행하도록 함.

- **결과**
```
--------- 계산기 Version 2.0 ---------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 1
첫 번째 숫자를 입력하세요: 3
사용할 연산자를 입력하세요: /
두 번째 숫자를 입력하세요: 0
[Error] 나눗셈 연산에서 분모가 0이 될 수 없습니다.
-------------------------------------
[ 3 / 0 = 정의되지 않음 ]
-------------------------------------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 1
-------------------------------------
첫 번째 숫자를 입력하세요: 2147483647
사용할 연산자를 입력하세요: +
두 번째 숫자를 입력하세요: 1
[Error] 연산 가능한 범위의 숫자를 초과했습니다.
-------------------------------------
[ 2147483647 + 1 = 연산 범위 초과 ]
-------------------------------------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 1
-------------------------------------
첫 번째 숫자를 입력하세요: 12345
사용할 연산자를 입력하세요: +
두 번째 숫자를 입력하세요: 12345
-------------------------------------
[ 12345 + 12345 = 24690 ]
-------------------------------------
```

---

## 실행 예시

- **정상 실행 예시**

```
--------- 계산기 Version 2.0 ---------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 1
-------------------------------------
첫 번째 숫자를 입력하세요: 98
사용할 연산자를 입력하세요: +
두 번째 숫자를 입력하세요: 456
-------------------------------------
[ 98 + 456 = 554 ]
-------------------------------------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 1
-------------------------------------
첫 번째 숫자를 입력하세요: 456
사용할 연산자를 입력하세요: /
두 번째 숫자를 입력하세요: 454
-------------------------------------
[ 456 / 454 = 1.00440529 ]
-------------------------------------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 2
-------------------------------------
[1] [ 98 + 456 = 554 ]
[2] [ 456 / 454 = 1.00440529 ]
-------------------------------------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 3
-------------------------------------
[1] [ 98 + 456 = 554 ]
[2] [ 456 / 454 = 1.00440529 ]
-------------------------------------
삭제할 기록의 번호를 입력하세요.
[취소: enter][전체: all 입력] 1
계산 기록이 삭제되었습니다.
-------------------------------------
[1] 사칙연산 계산하기
[2] 최근 계산 기록 조회하기
[3] 계산 기록 삭제하기
[4] 종료하기
실행할 메뉴 번호를 입력하세요: 4
------------- 계산기 종료 -------------
```


- **오류 실행 예시**

```
-------------------------------------
첫 번째 숫자를 입력하세요: 23
사용할 연산자를 입력하세요: /
두 번째 숫자를 입력하세요: 0
[Error] 나눗셈 연산에서 분모가 0이 될 수 없습니다.
-------------------------------------
사용할 연산자를 입력하세요: ,
[Error] 잘못된 입력이거나 지원하지 않는 연산자입니다.
-------------------------------------
두 번째 숫자를 입력하세요: 
[Error] 값이 입력되지 않았습니다.
-------------------------------------
두 번째 숫자를 입력하세요: 2147483648
[Error] 연산 가능한 범위의 숫자를 초과했습니다.
-------------------------------------
두 번째 숫자를 입력하세요: -1
[Error] 0 이상의 정수만 입력할 수 있습니다.
-------------------------------------
[1] [ 23 / 0 = 정의되지 않음 ]
[2] [ 1 + 23 = 24 ]
-------------------------------------
삭제할 기록의 번호를 입력하세요.
[취소: enter][전체: all 입력] a
[Error] 잘못된 입력입니다. 올바른 번호를 입력하세요.
-------------------------------------
```
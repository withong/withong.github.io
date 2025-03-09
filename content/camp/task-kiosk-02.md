+++
title = "[내일배움캠프] 과제 - 키오스크 2"
date = "2025-03-07T22:00:00+09:00"
draft = true
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "과제", "키오스크"]
+++

---

## 요구사항

### Lv 2. 객체 지향 설계를 적용해 햄버거 메뉴를 클래스로 관리하기

- **요구사항이 가지는 의도**
    - 객체 지향 개념을 학습하고, 데이터를 구조적으로 관리하며 프로그램을 설계하는 방법을 익힙니다.
    - 햄버거 메뉴를 `MenuItem` 클래스와 `List`를 통해 관리합니다.

- **`MenuItem` 클래스 생성하기**
    - 설명 : 개별 음식 항목을 관리하는 클래스입니다. 현재는 햄버거만 관리합니다.
    - 클래스는 `이름`, `가격`, `설명` 필드를 갖습니다.
- `main` 함수에서 `MenuItem` 클래스를 활용하여 햄버거 메뉴를 출력합니다.
    - `MenuItem` 객체 생성을 통해 `이름`, `가격`, `설명`을 세팅합니다.
        - 키워드: `new`
    - `List`를 선언하여 여러 `MenuItem`을 추가합니다.
        - `List<MenuItem> menuItems = new ArrayList<>();`
    - 반복문을 활용해 `menuItems`를 탐색하면서 하나씩 접근합니다.  

---

## 기능 구현

### Main.java

- **main()**  
    - 프로그램 시작 → 메뉴 출력
    - 사용자 입력 → 메뉴 선택 (잘못된 입력 시 재입력 요청)
    - 선택한 메뉴 확인 → 결제 여부 선택
    - 사용자가 지불 금액 입력 → 결제 진행
    - 결제 성공 → 잔액 출력 후 다시 메뉴 선택 진행
    - 결제 실패 → 금액 부족 메시지 출력 후 다시 메뉴 선택 진행
    - 사용자가 `0` 입력 시 프로그램 종료

```java
```

<br>

- **getInteger()**
    - 사용자 입력 값 검증
    - 숫자 입력 시 정수로 변환하여 반환
    - 숫자가 아닌 값 입력 시 오류 메시지 출력 후 재입력 요청

```java
public static int getInteger(Scanner scanner, String prompt) {
    while (true) {
        System.out.print(prompt);
        String input = scanner.nextLine();
        try {
            return Integer.parseInt(input);
        } catch (NumberFormatException e) {
            System.out.println("숫자만 입력 가능합니다.");
        }
    }
}
```

<br>

- **getDisplayBurgerName()**  
    - 메뉴 이름을 한글 기준으로 정렬 후 공백을 포함한 문자열을 반환

```java
public static String getDisplayBurgerName(List<List<String>> menu, String burgerName) {
    int maxLength = 0; // 최대 길이 계산
    for (List<String> item : menu) {
        maxLength = Math.max(maxLength, getDisplayLength(item.get(0)));
    }

    int paddingSize = maxLength - getDisplayLength(burgerName); // 부족한 공백 개수 계산
    return burgerName + " ".repeat(paddingSize); // 버거명 + 공백 리턴
}
```

<br>

- **getDisplayLength()**  
    - 한글을 2칸, 영어/숫자를 1칸으로 계산하여 글자 수 반환

```java
public static int getDisplayLength(String text) {
    int length = 0;
    for (char c : text.toCharArray()) {
        if (Character.toString(c).matches("[가-힣]")) {
            length += 2; // 한글이면 2칸
        } else {
            length += 1; // 영어/숫자/기타 문자는 1칸
        }
    }
    return length;
}
```

---

## 실행 예시

```powershell
```

---
+++
title = "[내일배움캠프] 과제 - 키오스크 1"
date = "2025-03-07T21:00:00+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "과제", "키오스크"]
+++

---

## 요구사항

### Lv 1. 기본적인 키오스크를 프로그래밍해보자

- **요구사항이 가지는 의도**
    - 입력 처리와 간단한 흐름 제어를 복습합니다. (프로그래밍 검증)
    - `Scanner` 활용법, 조건문, 반복문을 재확인하며 입력 데이터를 처리하는 방법 강화

- **햄버거 메뉴 출력 및 선택하기**
    - 실행시 햄버거 메뉴가 표시되고, 이후 Scanner로 숫자를 입력받아서 메뉴를 선택할 수 있다.
    - 제시된 메뉴 중 입력받은 숫자에 따라 다른 로직을 실행하는 코드를 작성합니다.
    - 반복문을 이용해서 특정 번호가 입력되면 프로그램을 종료합니다.  

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
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    List<List<String>> menu = List.of(
            List.of("불고기 버거", "￦ 4,200", "불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합."),
            List.of("치즈 버거", "￦ 3,600", "맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거."),
            List.of("슈비 버거", "￦ 6,600", "탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거."),
            List.of("슈슈 버거", "￦ 5,500", "탱글한 통새우살이 가득한 슈슈 버거.")
    );

    while (true) {
        System.out.println("\n[ MENU - BURGER ]");

        for (int i = 0; i < menu.size(); i++) {
            List<String> burger = menu.get(i);
            String burgerName = getDisplayBurgerName(menu, burger.get(0));
            System.out.println((i + 1) + ". " + burgerName + "\t| " + burger.get(1) + " | " + burger.get(2));
        }
        System.out.println("0. 종료하기");

        int user = getInteger(scanner, "\n주문할 메뉴의 번호를 입력하세요: ");

        if (user < 0 || user > menu.size()) {
            System.out.println("올바른 메뉴 번호를 입력하세요.");
            continue;
        }

        if (user == 0) {
            System.out.println("프로그램을 종료합니다.");
            break;
        }

        List<String> userChoice = menu.get(user-1);
        String userBurgerName = userChoice.get(0);
        String userBurgerPrice = userChoice.get(1);

        System.out.println("\n[" + userBurgerName + "] " + userBurgerPrice);
        user = getInteger(scanner, "결제하시겠습니까? [예: 1, 아니오: 2] ");

        switch (user) {
            case 1 -> {
                user = getInteger(scanner, "\n지불할 금액을 입력하세요: ");

                String numericPrice = userBurgerPrice.replace("￦", "").replace(",", "").trim();
                int price = Integer.parseInt(numericPrice);

                if (user > price) {
                    System.out.println(String.format("\n[잔액] ￦ %,d", (user - price)));

                } else if (user < price) {
                    System.out.println("금액이 부족합니다. 결제가 취소됩니다.");
                    break;
                }

                System.out.println("결제가 완료되었습니다. 감사합니다.");
            }

            case 2 -> System.out.println("결제가 취소되었습니다.");

            default -> System.out.println("올바른 입력이 아닙니다. 결제가 취소됩니다.");
        }
    }
}
```

<br>

- **getDisplayBurgerName()**  
    - 메뉴 이름을 한글 기준으로 정렬 후 공백을 포함한 문자열을 반환하는 메서드

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
    - 한글을 2칸, 영어/숫자를 1칸으로 계산하는 메서드

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

```
[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 3

[슈비 버거] ￦ 6,600
결제하시겠습니까? [예: 1, 아니오: 2] 1

지불할 금액을 입력하세요: 7000

[잔액] ￦ 400
결제가 완료되었습니다. 감사합니다.

[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 4

[슈슈 버거] ￦ 5,500
결제하시겠습니까? [예: 1, 아니오: 2] 2
결제가 취소되었습니다.

[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 1

[불고기 버거] ￦ 4,200
결제하시겠습니까? [예: 1, 아니오: 2] 1

지불할 금액을 입력하세요: 4200
결제가 완료되었습니다. 감사합니다.

[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 2

[치즈 버거] ￦ 3,600
결제하시겠습니까? [예: 1, 아니오: 2] 1

지불할 금액을 입력하세요: 3500
금액이 부족합니다. 결제가 취소됩니다.

[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 5
올바른 메뉴 번호를 입력하세요.

[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 4

[슈슈 버거] ￦ 5,500
결제하시겠습니까? [예: 1, 아니오: 2] 2
결제가 취소되었습니다.

[ MENU - BURGER ]
1. 불고기 버거	| ￦ 4,200 | 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
2. 치즈 버거  	| ￦ 3,600 | 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
3. 슈비 버거  	| ￦ 6,600 | 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
4. 슈슈 버거  	| ￦ 5,500 | 탱글한 통새우살이 가득한 슈슈 버거.
0. 종료하기

주문할 메뉴의 번호를 입력하세요: 0
프로그램을 종료합니다.
```

---
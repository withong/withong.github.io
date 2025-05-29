+++
title = "키오스크 필수 Lv 1-3"
date = "2025-03-07T21:00:00+09:00"
draft = false
tag = ["내일배움캠프", "과제"]
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

<br>

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

<br>

### Lv 3. 객체 지향 설계를 적용해 순서 제어를 클래스로 관리하기

- **요구사항이 가지는 의도**
    - 객체 지향 개념을 학습하고, 데이터를 구조적으로 관리하며 프로그램을 설계하는 방법을 익힙니다.
    - `main` 함수에서 관리하던 전체 순서 제어를 `Kiosk` 클래스를 통해 관리합니다.
- **`Kiosk` 클래스 생성하기**
    - **설명**: 키오스크 프로그램의 메뉴를 관리하고 사용자 입력을 처리하는 클래스입니다.
    - `MenuItem`을 관리하는 리스트가 필드로 존재합니다.
    - `main` 함수에서 관리하던 입력과 반복문 로직은 이제 `start` 함수를 만들어 관리합니다.
    - `List<MenuItem> menuItems` 는 `Kiosk` 클래스 생성자를 통해 값을 할당합니다.
        - `Kiosk` 객체를 생성하고 사용하는 `main` 함수에서 객체를 생성할 때 값을 넘겨줍니다.
- 요구사항에 부합하는지 검토
    - 키오스크 프로그램을 시작하는 메서드가 구현되어야 합니다.
        - 콘솔에 햄버거 메뉴를 출력합니다.
        - 사용자의 입력을 받아 메뉴를 선택하거나 프로그램을 종료합니다.
        - 유효하지 않은 입력에 대해 오류 메시지를 출력합니다.
        - `0`을 입력하면 프로그램이 ‘뒤로가기’되거나 ‘종료’됩니다.

---

## 기능 구현

<br>

### Main.java

```java
public class Main {

    public static void main(String[] args) {
        List<MenuItem> menuItems = Arrays.asList(
                new MenuItem("불고기 버거", 4200, "불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합."),
                new MenuItem("치즈 버거", 3600, "맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거."),
                new MenuItem("슈비 버거", 6600, "탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거."),
                new MenuItem("슈슈 버거", 5500, "탱글한 통새우살이 가득한 슈슈 버거.")
        );

        Kiosk kiosk = new Kiosk(menuItems);
        kiosk.start();
    }

}
```

<br>

### MenuItem.java

```java
public class MenuItem {

    private String name;
    private int price;
    private String desc;

    MenuItem(String name, int price, String desc) {
        this.name = name;
        this.price = price;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }

    public String getDesc() {
        return desc;
    }
}
```

<br>

### Kiosk.java

```java
public class Kiosk {

    private List<MenuItem> menuItems;

    Kiosk(List<MenuItem> menuItems) {
        this.menuItems = menuItems;
    }

    void start() {
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\n[ MENU - BURGER ]");

            for (int i = 0; i < menuItems.size(); i++) {
                MenuItem menu = menuItems.get(i);
                String menuName = getDisplayBurgerName(menuItems, menu.getName());
                String formatPrice = String.format("￦ %,d", menu.getPrice());

                System.out.println((i + 1) + ". " + menuName + "\t| " + formatPrice + " | " + menu.getDesc());
            }
            System.out.println("0. 종료하기");

            int userInput = getInteger(scanner, "\n주문할 메뉴의 번호를 입력하세요: ");

            if (userInput < 0 || userInput > menuItems.size()) {
                System.out.println("올바른 메뉴 번호를 입력하세요.");
                continue;
            }

            if (userInput == 0) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }

            MenuItem userChoice = menuItems.get(userInput - 1);
            String userChoiceName = userChoice.getName();
            int userChoicePrice = userChoice.getPrice();
            String formatPrice = String.format("￦ %,d", userChoicePrice);

            userInput = getInteger(scanner, "\n[" + userChoiceName + "] " + formatPrice + "\n결제하시겠습니까? [예: 1, 아니오: 2] ");

            if (userInput == 1) {
                int input = getInteger(scanner, "\n지불할 금액을 입력하세요: ");

                if (input > userChoicePrice) {
                    System.out.println(String.format("\n[잔액] ￦ %,d", (input - userChoicePrice)));

                } else if (input < userChoicePrice) {
                    System.out.println("금액이 부족합니다. 결제가 취소됩니다.");
                    continue;
                }
                System.out.println("결제가 완료되었습니다. 감사합니다.");

            } else if (userInput == 2) {
                System.out.println("결제가 취소되었습니다.");
            } else {
                System.out.println("올바른 입력이 아닙니다. 결제가 취소됩니다.");
            }
        }
    }

    private static int getInteger(Scanner scanner, String prompt) {
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

    // 메뉴 이름을 한글 기준으로 정렬 후 공백을 포함한 문자열을 반환하는 메서드
    private static String getDisplayBurgerName(List<MenuItem> menuItems, String menuName) {
        int maxLength = 0; // 최대 길이 계산
        for (MenuItem item : menuItems) {
            maxLength = Math.max(maxLength, getDisplayLength(item.getName()));
        }

        int paddingSize = maxLength - getDisplayLength(menuName); // 부족한 공백 개수 계산
        return menuName + " ".repeat(paddingSize); // 버거명 + 공백 리턴
    }

    // 한글을 2칸, 영어/숫자를 1칸으로 계산하는 메서드
    private static int getDisplayLength(String text) {
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
}
```

---

## 실행 예시

```powershell
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
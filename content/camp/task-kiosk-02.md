+++
title = "[내일배움캠프] 키오스크 Lv 4-5"
date = "2025-03-10T19:13:06+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "과제", "키오스크"]
+++

---

## 요구사항

### Lv 4. 객체 지향 설계를 적용해 음식 메뉴와 주문 내역을 클래스 기반으로 관리하기

- **`Menu` 클래스 생성하기**
    - 설명 : MenuItem 클래스를 관리하는 클래스입니다. 
    예를 들어, 버거 메뉴, 음료 메뉴 등 각 카테고리 내에 여러 `MenuItem`을 포함합니다.
    - `List<MenuItem>` 은 `Kiosk` 클래스가 관리하기에 적절하지 않으므로 Menu 클래스가 관리하도록 변경합니다.
    - 여러 버거들을 포함하는 상위 개념 ‘버거’ 같은 `카테고리 이름` 필드를 갖습니다.
    - 메뉴 카테고리 이름을 반환하는 메서드가 구현되어야 합니다.

<br>

### Lv 5. 캡슐화 적용하기

- `MenuItem`, `Menu` 그리고 `Kiosk` 클래스의 필드에 직접 접근하지 못하도록 설정합니다.
- Getter와 Setter 메서드를 사용해 데이터를 관리합니다.

---

## 구현 기능

<br>

### `Main.java` 메인 프로그램
- `List<Menu>`를 생성하여 메뉴 카테고리를 추가
- `new Menu()`를 생성하고 `.addMenuItems()`를 사용해 카테고리별 메뉴 추가
- `Kiosk` 객체를 생성하고 `start()`를 호출하여 프로그램 실행

```java
List<Menu> menu = Arrays.asList(
    new Menu("햄버거").addMenuItems(
            new MenuItem("불고기 버거", 4200, "불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합."),
            new MenuItem("치즈 버거", 3600, "맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거."),
            new MenuItem("슈비 버거", 6600, "탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거."),
            new MenuItem("슈슈 버거", 5500, "탱글한 통새우살이 가득한 슈슈 버거.")
    ),
    new Menu("음료").addMenuItems(
            new MenuItem("코카콜라", 2600, "갈증 해소뿐만이 아니라 기분까지 상쾌하게"),
            new MenuItem("스프라이트", 2600, "청량함에 레몬, 라임향을 더한 시원함"),
            new MenuItem("아메리카노", 3300, "바로 내린 100% 친환경 커피로 더 신선하게")
    ),
    new Menu("사이드").addMenuItems(
            new MenuItem("코카콜라", 2600, "갈증 해소뿐만이 아니라 기분까지 상쾌하게"),
            new MenuItem("스프라이트", 2600, "청량함에 레몬, 라임향을 더한 시원함"),
            new MenuItem("아메리카노", 3300, "바로 내린 100% 친환경 커피로 더 신선하게")
    )
);

Kiosk kiosk = new Kiosk(menu);
kiosk.start();
```

<br>

### `MenuItem.java` 메뉴 관리
- 메뉴 상세 정보 저장
- 메뉴명, 가격, 메뉴 설명을 포함하는 객체

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

### `Menu.java` 메뉴 카테고리 관리
- 카테고리에 속하는 메뉴 리스트 관리
- `addMenuItems(MenuItem... menu)`를 사용해 여러 개의 메뉴를 한 번에 추가

```java
public class Menu {

    private String category;
    private List<MenuItem> menuItems;

    Menu(String category) {
        this.category = category;
        menuItems = new ArrayList<>();
    }

    public String getCategory() {
        return category;
    }

    public List<MenuItem> getMenuItems() {
        return menuItems;
    }

    public Menu addMenuItems(MenuItem... menu) {
        this.menuItems.addAll(Arrays.asList(menu));
        return this;
    }
}
```

<br>

### `Kiosk.java` 키오스크 프로그램

- **`start()`**
    - 메인 메뉴를 출력하고, 사용자 입력을 받아 `order()` 호출
    - `0` 입력 시 프로그램 종료

```java
void start() {
    while (true) {

        System.out.println("\n☰ 메뉴");
        for (int i = 0; i < menus.size(); i++) {
            System.out.println(" [" + (i + 1) + "] " + menus.get(i).getCategory());
        }

        System.out.println(" [0] 종료하기");

        int userInput = getInteger();
        if (userInput == -1) continue;

        if (userInput < 0 || userInput > menus.size()) {
            System.out.println(" 올바른 메뉴 번호를 입력하세요.");
            continue;
        }

        if (userInput == 0) {
            System.out.println("프로그램을 종료합니다.");
            break;
        }

        Menu userChoice = menus.get(userInput - 1);
        order(userChoice);
    }
}
```

<br>

- **`order()`**
    - 메뉴 출력 및 선택
    - 선택한 카테고리에 포함된 메뉴를 출력
    - 메뉴 선택 시 `pay()` 호출하여 결제 진행
    - `0` 입력 시 뒤로 가기

```java
private void order(Menu userChoice) {
    List<MenuItem> menuItems = userChoice.getMenuItems();

    while (true) {
        System.out.println(getDisplayMenu(userChoice));

        int userInput = getInteger();
        if (userInput == -1) continue;

        if (userInput == 0) {
            break;
        }

        if (userInput < 0 || userInput > menuItems.size()) {
            System.out.println(" 올바른 메뉴 번호를 입력하세요.");
            continue;
        }

        MenuItem menuItem = menuItems.get(userInput - 1);
        int result = pay(menuItem);
        if (result == 0) break;
    }
}
```

<br>

- **`pay()`**
    - 선택한 메뉴 가격을 출력하고, 결제 여부 확인
    - 사용자가 금액을 입력하면 가격과 비교하여 결제 성공 또는 실패 처리
    - 금액이 부족하면 결제 여부 재확인

```java
private int pay(MenuItem menuItem) {
    while (true) {
        String menuItemName = menuItem.getName();
        int price = menuItem.getPrice();

        String displayPrice = String.format("￦ %,d", price);
        System.out.println("\n☰ 메뉴 > 결제");
        System.out.println(" [ " + menuItemName + " ] " + displayPrice);
        System.out.println(" 결제하시겠습니까? (예: 1, 아니오: 2)");
        int input = getInteger();

        switch (input) {
            case 1 -> {
                System.out.println("\n (결제중...) 금액을 입력하세요.");
                int amount = getInteger();

                if (amount < price) {
                    System.out.println(" 금액이 부족합니다.");
                    continue;
                } else if (amount > price) {
                    System.out.println(String.format("\n [잔액] ￦ %,d", (amount - price)));
                }
                System.out.println(" 결제가 완료되었습니다. 감사합니다.");
                return 0; // 0:홈
            }
            case 2 -> {
                System.out.println(" 결제가 취소되었습니다.");
                return 0;
            }

            case -1 -> {}
            default -> System.out.println(" 올바른 메뉴 번호를 입력하세요.");
        }
    }
}
```

---

## 구현 과정

<br>

### 메뉴 추가 방식 개선

- `Menu` 클래스가 추가되면서 `Main`에서의 초기화 코드가 복잡해지고 가독성이 떨어짐.
- `Menu` 객체 생성 후 `get(index)`를 사용해 카테고리별 메뉴를 추가할 경우, 어느 카테고리에 어떤 메뉴가 추가되는지 직관적으로 파악하기 어려움.
- `setMenuItems()` 대신 가변 인자를 갖는 `addMenuItems()`를 추가하고, `return this`를 활용해 **메서드 체이닝**이 가능하도록 개선

[변경 전]  

```java
List<Menu> menu = Arrays.asList(
    new Menu("햄버거"),
    new Menu("음료"),
    new Menu("사이드")
);

menu.get(0).setMenuItems(Arrays.asList(
    new MenuItem("불고기 버거", 4200, "불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합."),
    new MenuItem("치즈 버거", 3600, "맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거."),
    new MenuItem("슈비 버거", 6600, "탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거."),
    new MenuItem("슈슈 버거", 5500, "탱글한 통새우살이 가득한 슈슈 버거.")
));

menu.get(1).setMenuItems(Arrays.asList(
    new MenuItem("코카콜라", 2600, "갈증 해소뿐만이 아니라 기분까지 상쾌하게"),
    new MenuItem("스프라이트", 2600, "청량함에 레몬, 라임향을 더한 시원함"),
    new MenuItem("아메리카노", 3300, "바로 내린 100% 친환경 커피로 더 신선하게")
));

menu.get(2).setMenuItems(Arrays.asList(
    new MenuItem("후렌치 후라이", 3000, "남다른 맛과 바삭함"),
    new MenuItem("코울슬로", 2700, "아삭하게 씹히는 샐러드"),
    new MenuItem("치즈스틱", 3600, "속이 꽉 찬 황금빛 바삭함")
));
```

<br>

[변경 후]

```java
List<Menu> menu = Arrays.asList(
    new Menu("햄버거").addMenuItems(
            new MenuItem("불고기 버거", 4200, "불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합."),
            new MenuItem("치즈 버거", 3600, "맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거."),
            new MenuItem("슈비 버거", 6600, "탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거."),
            new MenuItem("슈슈 버거", 5500, "탱글한 통새우살이 가득한 슈슈 버거.")
    ),
    new Menu("음료").addMenuItems(
            new MenuItem("코카콜라", 2600, "갈증 해소뿐만이 아니라 기분까지 상쾌하게"),
            new MenuItem("스프라이트", 2600, "청량함에 레몬, 라임향을 더한 시원함"),
            new MenuItem("아메리카노", 3300, "바로 내린 100% 친환경 커피로 더 신선하게")
    ),
    new Menu("사이드").addMenuItems(
            new MenuItem("후렌치 후라이", 3000, "남다른 맛과 바삭함"),
            new MenuItem("코울슬로", 2700, "아삭하게 씹히는 샐러드"),
            new MenuItem("치즈스틱", 3600, "속이 꽉 찬 황금빛 바삭함")
    )
);
```
---

## 실행 예시

```powershell
☰ 메뉴
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 1

☰ 메뉴 > 햄버거
 [1] 불고기 버거	| ￦ 4,200	| 불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
 [2] 치즈 버거  	| ￦ 3,600	| 맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
 [3] 슈비 버거  	| ￦ 6,600	| 탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
 [4] 슈슈 버거  	| ￦ 5,500	| 탱글한 통새우살이 가득한 슈슈 버거.
 [0] 뒤로 가기
 ⮞ 3

☰ 메뉴 > 결제
 [ 슈비 버거 ] ￦ 6,600
 결제하시겠습니까? (예: 1, 아니오: 2)
 ⮞ 1

 (결제중...) 금액을 입력하세요.
 ⮞ 10000

 [잔액] ￦ 3,400
 결제가 완료되었습니다. 감사합니다.

☰ 메뉴
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 2

☰ 메뉴 > 음료
 [1] 코카콜라  	| ￦ 2,600	| 갈증 해소뿐만이 아니라 기분까지 상쾌하게
 [2] 스프라이트	| ￦ 2,600	| 청량함에 레몬, 라임향을 더한 시원함
 [3] 아메리카노	| ￦ 3,300	| 바로 내린 100% 친환경 커피로 더 신선하게
 [0] 뒤로 가기
 ⮞ 0

☰ 메뉴
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 3

☰ 메뉴 > 사이드
 [1] 후렌치 후라이	| ￦ 3,000	| 남다른 맛과 바삭함
 [2] 코울슬로     	| ￦ 2,700	| 아삭하게 씹히는 샐러드
 [3] 치즈스틱     	| ￦ 3,600	| 속이 꽉 찬 황금빛 바삭함
 [0] 뒤로 가기
 ⮞ 3

☰ 메뉴 > 결제
 [ 치즈스틱 ] ￦ 3,600
 결제하시겠습니까? (예: 1, 아니오: 2)
 ⮞ 2
 결제가 취소되었습니다.

☰ 메뉴
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 3

☰ 메뉴 > 사이드
 [1] 후렌치 후라이	| ￦ 3,000	| 남다른 맛과 바삭함
 [2] 코울슬로     	| ￦ 2,700	| 아삭하게 씹히는 샐러드
 [3] 치즈스틱     	| ￦ 3,600	| 속이 꽉 찬 황금빛 바삭함
 [0] 뒤로 가기
 ⮞ 2

☰ 메뉴 > 결제
 [ 코울슬로 ] ￦ 2,700
 결제하시겠습니까? (예: 1, 아니오: 2)
 ⮞ 1

 (결제중...) 금액을 입력하세요.
 ⮞ 2500
 금액이 부족합니다.

☰ 메뉴 > 결제
 [ 코울슬로 ] ￦ 2,700
 결제하시겠습니까? (예: 1, 아니오: 2)
 ⮞ 1

 (결제중...) 금액을 입력하세요.
 ⮞ 2700
 결제가 완료되었습니다. 감사합니다.

☰ 메뉴
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 2

☰ 메뉴 > 음료
 [1] 코카콜라  	| ￦ 2,600	| 갈증 해소뿐만이 아니라 기분까지 상쾌하게
 [2] 스프라이트	| ￦ 2,600	| 청량함에 레몬, 라임향을 더한 시원함
 [3] 아메리카노	| ￦ 3,300	| 바로 내린 100% 친환경 커피로 더 신선하게
 [0] 뒤로 가기
 ⮞ 0

☰ 메뉴
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 0

프로그램을 종료합니다.
```

---
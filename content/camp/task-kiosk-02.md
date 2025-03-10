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

## 기능 구현

<br>

### Main.java

```java
public class Main {

    public static void main(String[] args) {
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

### Menu.java

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

### Kiosk.java

```java
public class Kiosk {

    private List<Menu> menus;
    private Scanner scanner = new Scanner(System.in);

    Kiosk(List<Menu> menu) {
        this.menus = menu;
    }

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

    private int getInteger() {
        System.out.print(" ⮞ ");
        String input = scanner.nextLine();
        try {
            return Integer.parseInt(input);
        } catch (NumberFormatException e) {
            System.out.println(" 유효한 입력이 아닙니다.");
            return -1;
        }
    }

    private String getDisplayMenu(Menu menu) {
        StringBuilder sb = new StringBuilder();
        List<MenuItem> menuItems = menu.getMenuItems();
        String category = menu.getCategory();

        sb.append( "\n☰ 메뉴 > " + category + "\n");

        for (int i = 0; i < menuItems.size(); i++) {
            MenuItem item = menuItems.get(i);

            String menuName = getDisplayMenuName(menuItems, item.getName());
            String formatPrice = String.format("￦ %,d", item.getPrice());
            sb.append(" [" + (i + 1) + "] " + menuName + "\t| " + formatPrice + "\t| " + item.getDesc()+ "\n");
        }
        sb.append(" [0] 뒤로 가기");

        return sb.toString();
    }

    // 메뉴 이름을 한글 기준으로 정렬 후 공백을 포함한 문자열을 반환하는 메서드
    private String getDisplayMenuName(List<MenuItem> menuItems, String menuName) {
        int maxLength = 0; // 최대 길이 계산
        for (MenuItem item : menuItems) {
            maxLength = Math.max(maxLength, getDisplayLength(item.getName()));
        }

        int paddingSize = maxLength - getDisplayLength(menuName); // 부족한 공백 개수 계산
        return menuName + " ".repeat(paddingSize); // 버거명 + 공백 리턴
    }

    // 한글을 2칸, 영어/숫자를 1칸으로 계산하는 메서드
    private int getDisplayLength(String text) {
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
+++
title = "키오스크 도전 Lv 2"
date = "2025-03-12T21:01:14+09:00"
draft = false
topic = ["task"]
tag = ["내일배움캠프", "키오스크"]
+++

---

## 요구사항

### Lv 2. Enum, 람다 & 스트림을 활용한 주문 및 장바구니 관리

- **요구사항이 가지는 의도**
    - **의도** : 고급 자바 기능을 활용해 프로그램의 효율성과 코드의 가독성을 개선하는 것을 목표로 합니다.
    - **목적**
        - Enum을 통해 상수를 안전하게 관리하고, 프로그램 구조를 간결하게
        - 제네릭을 활용하여 데이터 유연성을 높이고, 재사용 가능한 코드를 설계
        - 스트림 API를 사용하여 데이터를 필터링하고, 간결한 코드로 동작을 구현
- **Enum을 활용한 사용자 유형별 할인율 관리하기**
    - 사용자 유형의 Enum 정의 및 각 사용자 유형에 따른 할인율 적용
        - 예시 : 군인, 학생, 일반인
    - 주문 시, 사용자 유형에 맞는 할인율 적용해 총 금액 계산
- **람다 & 스트림을 활용한 장바구니 조회 기능**
    - 기존에 생성한 Menu의 MenuItem을 조회 할 때 스트림을 사용하여 출력하도록 수정
    - 기존 장바구니에서 특정 메뉴 빼기 기능을 통한 스트림 활용
        - 예시 : 장바구니에 SmokeShack 가 들어 있다면, stream.filter를 활용하여 특정 메뉴 이름을 가진 메뉴 장바구니에서 제거
---

## 구현 기능
[# 전체 코드](https://github.com/withong/nbc-chapter-02/tree/main/kiosk/challenge/level2)

### 할인 기능 추가

- `enum Discount` 추가

```java
public enum Discount {
    BIRTHDAY("생일",0.2),
    REVIEW("리뷰 작성", 0.1),
    SNS("SNS 업로드", 0.1),
    STUDENT("학생", 0.05);

    private final String name;
    private final double rate;

    Discount(String name, double rate) {
        this.name = name;
        this.rate = rate;
    }

    public int getDiscounted(int price) {
        return price - (int) (price * rate);
    }

    public String getName() {
        return name;
    }

    public double getRate() {
        return rate;
    }
}
```

<br>

- 할인 메뉴 리스트 포맷팅하여 반환하는 메서드 추가

```java
private String getDisplayDiscount() {
    Discount[] discounts = Discount.values();

    String discountList = IntStream.range(0, discounts.length)
                            .mapToObj(i -> String.format(" [%d] %s\t\uD83D\uDD25 %.0f%%",
                                    i + 1,
                                    getDisplayName(discounts[i].getName()),
                                    discounts[i].getRate() * 100))
                            .collect(Collectors.joining("\n"));

    return "\n☰ 홈 > 결제 > 할인\n"
            + discountList
            + "\n [" + (discounts.length + 1)
            + "] 해당 없음\n [0] 홈으로";
}
```

<br>

- Kiosk 클래스에 discount() 메서드 추가
    - 할인 대상인지 확인 후 할인율 적용된 가격 담아서 결제 메서드 호출
```java
private int discount() {
    while (true) {
        int price = cart.getTotalPrice();
        Discount[] discounts = Discount.values();
        int lastNumber = Discount.values().length + 1; // 해당 없음

        // 할인 메뉴 출력
        System.out.println(getDisplayDiscount());

        // 사용자 입력 값 검증
        int input = getInteger();
        // -1 반환 시 재입력 요청
        if (input == -1) continue;

        if (input == 0) {
            System.out.println(" 결제가 취소되었습니다.");
            return 0;
        }

        if (input < 1 || input > lastNumber) {
            System.out.println(" 올바른 메뉴 번호를 입력하세요.");
            continue;
        }

        if (input == lastNumber) {
            pay(price);
        }

        int discountedPrice = discounts[input-1].getDiscounted(price);
        int result = pay(discountedPrice);
        if (result == 0) return 0;
    }
}
```

<br>

### 람다 & 스트림 활용

- 장바구니 목록 삭제

[변경 전]
```java
if (quantity - input < 1 || cartItem.getQuantity() < 1) {
    cart.getCartItems().remove(cartItem);
}

```

<br>

[변경 후]
```java
cart.setCartItems(cart.getCartItems().stream()
            .filter(item -> item.getQuantity() > 0)
            .collect(Collectors.toList()));
```

<br>

[변경 전]
```java
cart.getCartItems().remove(cartItem);
```

<br>

[변경 후]
```java
// 수량이 1개면 바로 삭제
cart.setCartItems(cart.getCartItems().stream()
        .filter(item -> !item.equals(cartItem))
        .collect(Collectors.toList()));
```

<br>

- 카테고리별 메뉴 출력

[변경 전]
```java
private String getDisplayMenu(Menu menu) {
    StringBuilder sb = new StringBuilder();
    List<MenuItem> menuItems = menu.getMenuItems();
    String category = menu.getCategory();

    sb.append("\n☰ 홈 > " + category + "\n");

    for (int i = 0; i < menuItems.size(); i++) {
        MenuItem item = menuItems.get(i);

        String menuName = getDisplayName(item.getName());
        String formatPrice = String.format("￦ %,d", item.getPrice());
        sb.append(" [" + (i + 1) + "] " + menuName + "\t|  " + formatPrice + "   |   " + item.getDesc() + "\n");
    }
    sb.append("\n [0] 홈으로");

    int index = menus.indexOf(menu);
    // 메뉴 네비게이션 바 조건: 첫 번째 메뉴면 다음 메뉴만, 마지막 메뉴면 이전 메뉴만, 아니면 모두 출력
    if (index == 0) {
        sb.append("    [n] 다음 메뉴");
    } else if (index == (menus.size() - 1)) {
        sb.append("    [p] 이전 메뉴");
    } else if (0 < index || index < (menus.size() - 1)) {
        sb.append("    [p] 이전 메뉴    [n] 다음 메뉴");
    }
    // 장바구니가 비어 있지 않을 때만 네비게이션 바에 출력
    if (!cart.isEmpty()) {
        sb.append("    [c] 장바구니");
    }

    return sb.toString();
}
```

<br>

[변경 후]
```java
private String getDisplayMenu(Menu menu) {
    List<MenuItem> menuItems = menu.getMenuItems();
    String category = menu.getCategory();

    String menuList = menuItems.stream()
            .map(item -> String.format(" [%d] %s\t|  ￦ %,d   |   %s",
                    menuItems.indexOf(item) + 1,
                    getDisplayName(item.getName()),
                    item.getPrice(),
                    item.getDesc()))
            .collect(Collectors.joining("\n"));

    int index = menus.indexOf(menu);
    String navigation = "\n [0] 홈으로";

    // 메뉴 네비게이션 바 조건: 첫 번째 메뉴면 다음 메뉴만, 마지막 메뉴면 이전 메뉴만, 아니면 모두 출력
    if (index == 0) {
        navigation += "    [n] 다음 메뉴";
    } else if (index == (menus.size() - 1)) {
        navigation += "    [p] 이전 메뉴";
    } else if (0 < index || index < (menus.size() - 1)) {
        navigation += "    [p] 이전 메뉴    [n] 다음 메뉴";
    }
    // 장바구니가 비어 있지 않을 때만 네비게이션 바에 출력
    if (!cart.isEmpty()) {
        navigation += "    [c] 장바구니";
    }

    return "\n☰ 홈 > " + category
            + "\n" + menuList
            + navigation;
}
```

<br>

- 장바구니 목록 출력

[변경 전]
```java
private String getDisplayCartItems() {
    StringBuilder sb = new StringBuilder();
    List<CartItem> cartItems = cart.getCartItems();

    sb.append("\n☰ 홈 > 장바구니\n");
    sb.append("--------------------------------------\n");

    for (int i = 0; i < cartItems.size(); i++) {
        CartItem item = cartItems.get(i);

        String itemName = getDisplayName(item.getMenuItem().getName());
        String formatPrice = String.format("￦ %,d", item.getTotalPrice());
        sb.append(" [" + (i + 1) + "] " + itemName + "\t×  " + item.getQuantity() + "  =  " + formatPrice + "\n");
    }

    String formatTotalPrice = String.format("￦ %,d", cart.getTotalPrice());

    sb.append("--------------------------------------\n");
    sb.append(" 총 결제 금액\t\t\t\t\t " + formatTotalPrice);

    return sb.toString();
}
```

<br>

[변경 후]
```java
private String getDisplayCartItems() {
    List<CartItem> cartItems = cart.getCartItems();

    String line ="--------------------------------------\n";

    String cartList = cartItems.stream()
            .filter(item -> item.getQuantity() > 0)
            .map(item -> String.format(" [%d] %s\t×  %d  =  ￦ %,d",
                    cartItems.indexOf(item) + 1,
                    getDisplayName(item.getMenuItem().getName()),
                    item.getQuantity(),
                    item.getTotalPrice()))
            .collect(Collectors.joining("\n"));

    String formatTotalPrice = String.format("￦ %,d", cart.getTotalPrice());
    return "\n☰ 홈 > 장바구니\n"
            + line + cartList
            + "\n" + line
            + " 총 결제 금액     \t      =  " + formatTotalPrice;
}
```

<br>

- 할인 대상 목록

[변경 전]
```java
private String getDisplayDiscount() {
    StringBuilder sb = new StringBuilder();
    Discount[] discounts = Discount.values();

    sb.append("\n☰ 홈 > 결제 > 할인\n");

    for (int i = 0; i < discounts.length; i++) {
        String displayName = getDisplayName(discounts[i].getName());
        String displayRate = String.format("%.0f%%", discounts[i].getRate() * 100);

        sb.append(" [" + (i + 1) + "] " + displayName + "\t\uD83D\uDD25 " + displayRate + "\n");
    }
    sb.append(" [" + (discounts.length + 1) + "] 해당 없음\n");
    sb.append(" [0] 홈으로");

    return sb.toString();
}
```

<br>

[변경 후]
```java
private String getDisplayDiscount() {
    Discount[] discounts = Discount.values();

    String discountList = IntStream.range(0, discounts.length)
                    .mapToObj(i -> String.format(" [%d] %s\t\uD83D\uDD25 %.0f%%",
                            i + 1,
                            getDisplayName(discounts[i].getName()),
                            discounts[i].getRate() * 100))
                    .collect(Collectors.joining("\n"));

    return "\n☰ 홈 > 결제 > 할인\n"
            + discountList
            + "\n [" + (discounts.length + 1)
            + "] 해당 없음\n [0] 홈으로";
}
```

---

## 구현 과정

### `toList()` vs `collect(Collectors.toList())`
- 장바구니 목록 삭제 기능을 스트림으로 수정하면서 처음에는 `toList()`를 사용하여 장바구니 리스트를 세팅함.
- 장바구니 목록 삭제 후 새로운 메뉴 추가를 시도하면 `UnsupportedOperationException` 예외 발생.
- `toList()`는 불변 리스트를, `collect(Collectors.toList())`는 가변 리스트를 반환함.
- 계속해서 메뉴 추가되고 삭제될 가능성이 있는 장바구니에는 가변 리스트가 적합.

깔끔해서 쓰고 싶었는데...🥲

---

## 실행 예시

```powershell
☰ 홈
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 1

☰ 홈 > 햄버거
 [1] 불고기 버거  	|  ￦ 4,200   |   불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
 [2] 치즈 버거    	|  ￦ 3,600   |   맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
 [3] 슈비 버거    	|  ￦ 6,600   |   탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
 [4] 슈슈 버거    	|  ￦ 5,500   |   탱글한 통새우살이 가득한 슈슈 버거.
 [0] 홈으로    [n] 다음 메뉴
 ⮞ 3

☰ 홈 > 햄버거
 [1] 불고기 버거  	|  ￦ 4,200   |   불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
 [2] 치즈 버거    	|  ￦ 3,600   |   맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
 [3] 슈비 버거    	|  ￦ 6,600   |   탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
 [4] 슈슈 버거    	|  ￦ 5,500   |   탱글한 통새우살이 가득한 슈슈 버거.
 [0] 홈으로    [n] 다음 메뉴    [c] 장바구니
 ⮞ n

☰ 홈 > 음료
 [1] 코카콜라     	|  ￦ 2,600   |   갈증 해소뿐만이 아니라 기분까지 상쾌하게
 [2] 스프라이트   	|  ￦ 2,600   |   청량함에 레몬, 라임향을 더한 시원함
 [3] 아메리카노   	|  ￦ 3,300   |   바로 내린 100% 친환경 커피로 더 신선하게
 [0] 홈으로    [p] 이전 메뉴    [n] 다음 메뉴    [c] 장바구니
 ⮞ 1

☰ 홈 > 음료
 [1] 코카콜라     	|  ￦ 2,600   |   갈증 해소뿐만이 아니라 기분까지 상쾌하게
 [2] 스프라이트   	|  ￦ 2,600   |   청량함에 레몬, 라임향을 더한 시원함
 [3] 아메리카노   	|  ￦ 3,300   |   바로 내린 100% 친환경 커피로 더 신선하게
 [0] 홈으로    [p] 이전 메뉴    [n] 다음 메뉴    [c] 장바구니
 ⮞ p

☰ 홈 > 햄버거
 [1] 불고기 버거  	|  ￦ 4,200   |   불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
 [2] 치즈 버거    	|  ￦ 3,600   |   맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
 [3] 슈비 버거    	|  ￦ 6,600   |   탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
 [4] 슈슈 버거    	|  ￦ 5,500   |   탱글한 통새우살이 가득한 슈슈 버거.
 [0] 홈으로    [n] 다음 메뉴    [c] 장바구니
 ⮞ 4

☰ 홈 > 햄버거
 [1] 불고기 버거  	|  ￦ 4,200   |   불고기 소스에 패티와 마요네즈, 양상추의 맛있는 조합.
 [2] 치즈 버거    	|  ￦ 3,600   |   맛있는 치즈와 100% 순 쇠고기 패티, 클래식 치즈 버거.
 [3] 슈비 버거    	|  ￦ 6,600   |   탱글한 통새우살에 비프 패티를 더해 푸짐한 슈비 버거.
 [4] 슈슈 버거    	|  ￦ 5,500   |   탱글한 통새우살이 가득한 슈슈 버거.
 [0] 홈으로    [n] 다음 메뉴    [c] 장바구니
 ⮞ n

☰ 홈 > 음료
 [1] 코카콜라     	|  ￦ 2,600   |   갈증 해소뿐만이 아니라 기분까지 상쾌하게
 [2] 스프라이트   	|  ￦ 2,600   |   청량함에 레몬, 라임향을 더한 시원함
 [3] 아메리카노   	|  ￦ 3,300   |   바로 내린 100% 친환경 커피로 더 신선하게
 [0] 홈으로    [p] 이전 메뉴    [n] 다음 메뉴    [c] 장바구니
 ⮞ n

☰ 홈 > 사이드
 [1] 후렌치 후라이	|  ￦ 3,000   |   남다른 맛과 바삭함
 [2] 코울슬로     	|  ￦ 2,700   |   아삭하게 씹히는 샐러드
 [3] 치즈스틱     	|  ￦ 3,600   |   속이 꽉 찬 황금빛 바삭함
 [0] 홈으로    [p] 이전 메뉴    [c] 장바구니
 ⮞ 1

☰ 홈 > 사이드
 [1] 후렌치 후라이	|  ￦ 3,000   |   남다른 맛과 바삭함
 [2] 코울슬로     	|  ￦ 2,700   |   아삭하게 씹히는 샐러드
 [3] 치즈스틱     	|  ￦ 3,600   |   속이 꽉 찬 황금빛 바삭함
 [0] 홈으로    [p] 이전 메뉴    [c] 장바구니
 ⮞ 3

☰ 홈 > 사이드
 [1] 후렌치 후라이	|  ￦ 3,000   |   남다른 맛과 바삭함
 [2] 코울슬로     	|  ￦ 2,700   |   아삭하게 씹히는 샐러드
 [3] 치즈스틱     	|  ￦ 3,600   |   속이 꽉 찬 황금빛 바삭함
 [0] 홈으로    [p] 이전 메뉴    [c] 장바구니
 ⮞ c

☰ 홈 > 장바구니
--------------------------------------
 [1] 슈비 버거    	×  1  =  ￦ 6,600
 [2] 코카콜라     	×  1  =  ￦ 2,600
 [3] 슈슈 버거    	×  1  =  ￦ 5,500
 [4] 후렌치 후라이	×  1  =  ￦ 3,000
 [5] 치즈스틱     	×  1  =  ￦ 3,600
--------------------------------------
 총 결제 금액     	      =  ￦ 21,300
--------------------------------------
 [1] 결제하기
 [2] 메뉴 삭제하기
 [3] 장바구니 비우기
 [0] 홈으로
 ⮞ 2

 삭제할 메뉴의 번호를 입력하세요.
 ⮞ 3

☰ 홈 > 장바구니
--------------------------------------
 [1] 슈비 버거    	×  1  =  ￦ 6,600
 [2] 코카콜라     	×  1  =  ￦ 2,600
 [3] 후렌치 후라이	×  1  =  ￦ 3,000
 [4] 치즈스틱     	×  1  =  ￦ 3,600
--------------------------------------
 총 결제 금액     	      =  ￦ 15,800
--------------------------------------
 [1] 결제하기
 [2] 메뉴 삭제하기
 [3] 장바구니 비우기
 [0] 홈으로
 ⮞ 1

☰ 홈 > 결제 > 할인
 [1] 생일         	🔥 20%
 [2] 리뷰 작성    	🔥 10%
 [3] SNS 업로드   	🔥 10%
 [4] 학생         	🔥 5%
 [5] 해당 없음
 [0] 홈으로
 ⮞ 1

☰ 홈 > 결제
 총 결제 금액 [ ￦ 12,640 ]
 지불할 금액을 입력하세요. (취소: 0)
 ⮞ 15000

 [잔액] ￦ 2,360
 결제가 완료되었습니다. 감사합니다.

☰ 홈
 [1] 햄버거
 [2] 음료
 [3] 사이드
 [0] 종료하기
 ⮞ 0

프로그램을 종료합니다.
```

---
+++
title = "[내일배움캠프] 키오스크 도전 Lv 1"
date = "2025-03-12T11:17:50+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "과제", "키오스크"]
+++

---

## 요구사항

### Lv 1. 장바구니 및 구매하기 기능을 추가하기

- **요구사항이 가지는 의도**
    - **의도**: 클래스 간 연계를 통해 객체 지향 프로그래밍의 기본적인 설계를 익히고, 사용자 입력에 따른 프로그램 흐름 제어와 상태 관리를 학습
    - **목표**
        - 클래스 간 역할 분리를 이해하고, 적절히 협력하는 객체를 설계
        - 프로그램 상태 변경 및 데이터 저장을 연습
        - 사용자 입력에 따른 예외 처리와 조건 분기를 연습
- **장바구니 생성 및 관리 기능**
    - 사용자가 선택한 메뉴를 장바구니에 추가할 수 있는 기능을 제공합니다.
    - 장바구니는 메뉴명, 수량, 가격 정보를 저장하며, 항목을 동적으로 추가 및 조회할 수 있어야 합니다.
    - 사용자가 잘못된 선택을 했을 경우 예외를 처리합니다(예: 유효하지 않은 메뉴 번호 입력)
- **장바구니 출력 및 금액 계산**
    - 사용자가 결제를 시도하기 전에, 장바구니에 담긴 모든 메뉴와 총 금액을 출력합니다.
    - 출력 예시
        - 각 메뉴의 이름, 가격, 수량
        - 총 금액 합계
- **장바구니 담기 기능**
    - 메뉴를 클릭하면 장바구니에 추가할 지 물어보고, 입력값에 따라 “추가”, “취소” 처리합니다.
        - 메뉴는 한 번에 1개만 담을 수 있습니다.
    - 장바구니에 담은 목록을 출력합니다.
- **주문 기능**
    - 장바구니에 담긴 모든 항목을 출력합니다.
    - 합산하여 총 금액을 계산하고, “주문하기”를 누르면 장바구니를 초기화합니다.

---

## 구현 기능
[# 전체 코드](https://github.com/withong/nbc-chapter-02/tree/main/kiosk/challenge/level1)

<br>

### 장바구니 아이템 관리
- `CartItem` 클래스 추가  
```java
public class CartItem {

    private MenuItem menuItem;
    private int quantity;

    CartItem(MenuItem menuItem) {
        this.menuItem = menuItem;
        this.quantity = 1;
    }

    public void addQuantity() {
        this.quantity++;
    }

    public MenuItem getMenuItem() {
        return menuItem;
    }

    public int getQuantity() {
        return quantity;
    }

    public void setQuantity(int quantity) {
        this.quantity = quantity;
    }

    public int getTotalPrice() {
        return menuItem.getPrice() * this.quantity;
    }
}
```

<br>

### 장바구니 관리
- `Cart` 클래스 추가   
```java
public class Cart {

    private List<CartItem> cartItems;

    Cart() {
        this.cartItems = new ArrayList<>();
    }

    public void addCartItem(MenuItem menuItem) {
        for (CartItem cartItem : cartItems) {
            if (cartItem.getMenuItem().getName().equals(menuItem.getName())) {
                cartItem.addQuantity();
                return;
            }
        }
        cartItems.add(new CartItem(menuItem));
    }

    public List<CartItem> getCartItems() {
        return cartItems;
    }

    public int getTotalPrice() {
        int total = 0;
        for (CartItem cartItem : cartItems) {
            total += cartItem.getTotalPrice();
        }
        return total;
    }

    public void clearCart() {
        cartItems.clear();
    }

    public boolean isEmpty() {
        return cartItems.isEmpty();
    }
}
```

<br>

### 키오스크 프로그램
- 메뉴 출력 시, 장바구니가 비어 있지 않으면 장바구니 메뉴도 출력
```java
int maxMenuNumber = menus.size();

System.out.println("\n☰ 홈");
// 카테고리 리스트(menus)의 카테고리명 가져와서 메뉴 출력
for (int i = 0; i < maxMenuNumber; i++) {
    System.out.println(" [" + (i + 1) + "] " + menus.get(i).getCategory());
}

if (!cart.isEmpty()) {
    maxMenuNumber++;
    System.out.println(" [" + maxMenuNumber + "] 장바구니");
}

System.out.println(" [0] 종료하기");
```

<br>

- `order()` 메서드에서 메뉴 선택 시 장바구니에 추가
```java
// 장바구니에 추가
cart.addCartItem(menuItem);
```

<br>

- `cart()` 메서드 추가
    - 결제, 메뉴 삭제, 장바구니 비우기, 홈으로 돌아가기 가능
    - 메뉴 삭제 시 수량이 2 이상인 경우 삭제할 수량을 입력받아서 진행
    - 결제하기 선택 시 `pay()` 메서드 호출
```java
private int cart() {
    if (cart.isEmpty()) {
        System.out.println("\n 장바구니에 담긴 상품이 없습니다.");
        return 0;
    }

    while (true) {
        System.out.println(getDisplayCartItems());
        System.out.println("--------------------------------------");
        System.out.println(" [1] 결제하기");
        System.out.println(" [2] 메뉴 삭제하기");
        System.out.println(" [3] 장바구니 비우기");
        System.out.println(" [0] 홈으로");

        int input = getInteger();

        switch (input) {
            case 1 -> {
                int result = pay();
                if (result == 0) return 0;
            }
            case 2 -> {
                System.out.println("\n 삭제할 메뉴의 번호를 입력하세요.");
                input = getInteger();

                int cartSize = cart.getCartItems().size();
                if (input < 1 || input > cartSize) {
                    System.out.println(" 올바른 메뉴 번호를 입력하세요.");
                    continue;
                }

                CartItem cartItem = cart.getCartItems().get(input - 1);
                int quantity = cartItem.getQuantity();

                if (quantity > 1) {
                    System.out.println("\n [" + cartItem.getMenuItem().getName() + "] 현재 수량 " + quantity + "개");
                    System.out.println(" 삭제할 수량을 입력하세요.");
                    input = getInteger();

                    if (input > quantity) {
                        System.out.println(" 수량보다 큰 숫자를 입력할 수 없습니다.");
                        continue;
                    }
                    cartItem.setQuantity(quantity - input);

                    if (quantity - input < 1 || cartItem.getQuantity() < 1) {
                        cart.getCartItems().remove(cartItem);
                    }
                } else {
                    cart.getCartItems().remove(cartItem);
                }
            }
            case 3 -> {
                cart.clearCart();
                return 0;
            }
            case 0 -> {
                return 0;
            }
        }
    }
}
```

<br>

- 메뉴 출력 시 네비게이션 바 추가
```java
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
```

<br>

- 주문 시 네비게이션 바 입력 값에 따른 처리
```java
// 메뉴 네비게이션 바 입력 값 검사
if (userInput == 112) { // p 이전 메뉴
    index = index - 1;
    continue;

} else if (userInput == 110) { // n 다음 메뉴
    index = index + 1;
    continue;

} else if (userInput == 99) { // c 장바구니
    if (cart.isEmpty()) {
        throw new ArrayIndexOutOfBoundsException();
    }
    int result = cart();
    if (result == 0) break;
}
```

<br>

- 예외 처리 추가
```java
catch (ArrayIndexOutOfBoundsException e) {
    // 메뉴 네비게이션 바 입력 값 예외 처리: 없는 인덱스거나, 빈 장바구니 호출 시 실행
    System.out.println(" 올바른 메뉴를 입력하세요.");
}
```
---

## 구현 과정

<br>

### 메뉴 네비게이션 바 추가
- 메뉴 하나를 선택할 때마다 홈으로 돌아갔다가 다시 메뉴 카테고리를 선택해서 들어가는 순서가 번거롭게 느껴졌음. 원래 방향키를 이용해서 이동하고 싶었는데 scanner는 방향키 입력을 인지할 수 없어서 int로 변환 가능한 char을 사용해 입력받기로 결정.
- 초기 order 메서드는 메뉴 카테고리와 메뉴 리스트를 불러오는 로직이 `while`문 밖에 있었음. 이 부분을 인지하지 못하고 네비게이션 메뉴를 입력받을 때 계속해서 order 메서드를 중복 호출함 → break; 해도 기존 order 메서드가 끝나지 않은 상태라 start 메서드로 돌아가지 못하는 문제 발생.
- 메뉴 카테고리와 메뉴 리스트를 가져오는 로직을 `while`문 시작으로 옮기고, 네비게이션 메뉴 입력 시 `index` 값을 변경하는 방식으로 수정.
- 메뉴 출력 시 카테고리의 인덱스를 확인하고 적절한 네비게이션 메뉴만을 출력하고 있지만, 실제 입력 시에는 없는 네비게이션 메뉴도 입력받을 수 있는 상태이기 때문에 존재하지 않는 인덱스를 호출할 때의의 예외 처리 추가.

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
 [3] 후렌치 후라이	×  1  =  ￦ 3,000
 [4] 치즈스틱     	×  1  =  ￦ 3,600
--------------------------------------
 총 결제 금액   			 ￦ 15,800
--------------------------------------
 [1] 결제하기
 [2] 메뉴 삭제하기
 [3] 장바구니 비우기
 [0] 홈으로
 ⮞ 1

☰ 홈 > 결제
 총 결제 금액 [ ￦ 15,800 ]
 지불할 금액을 입력하세요. (취소: 0)
 ⮞ 20000

 [잔액] ￦ 4,200
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
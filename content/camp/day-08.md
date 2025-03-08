+++
title = "[내일배움캠프] 8일차. Java 심화 1"
date = "2025-02-26T21:00:00+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "java"]
+++

> ✅ [강의] Java 문법 Chapter 3 (1~4)  
>✅ [과제] Java 계산기 만들기 level 1  

---

## 예외와 예외 처리
- **예외** → 프로그램 실행 중 발생할 수 있는 예기치 못한 오류나 상황. 예외가 처리되지 않으면 프로그램 흐름이 중단됨.

- **예외 구조와 종류**
    - **UncheckedException** 
    → `RuntimeException`과 그 하위 클래스. 예외 처리 강제되지 않음. 
    예: `NullPointerException`, `ArrayIndexOutOfBoundsException`
    - **CheckedException**
    → `RuntimeException`을 제외한 예외. 예외 처리 강제됨.
    예: `IOException`, `SQLException`.
        
- **예외 전파**
    - **UncheckedException** 
    → 예외 전파가 자동으로 이루어짐.
    → `main()` 메서드나 스레드에서 처리하지 않으면 프로그램이 종료됨.
    - **CheckedException** 
    → 예외가 발생했을 때 반드시 예외를 처리하거나 전파해야 함.
    → `try-catch`문 또는 메서드 선언에 `throws` 키워드 사용.

<br>

## Optional
- **`Optional<T>`** → **Null을 안전하게 다루기 위한 클래스**
  - 값이 존재할 수도 있고, 없을 수도 있는 객체를 감싸는 컨테이너 역할
  - `NullPointerException`을 방지하기 위해 사용됨.
  - `null` 체크를 명시적으로 하지 않고도 값의 존재 여부를 쉽게 처리할 수 있음.

- **`Optional.ofNullable(T value)`** → `null`일 수도 있는 값을 감싼다.

  ```java
  Optional<String> opt = Optional.ofNullable(null); // 빈 Optional 생성
  ```

- **`isPresent()`** → 값이 존재하면 `true`, 없으면 `false` 반환.

- **`Optional<T>.get()`** → `Optional` 객체에 값이 존재할 때 그 값을 반환. 
값이 없으면 `NoSuchElementException` 예외를 발생시킴.

  ```java
  Optional<String> optional = Optional.of("Hello");

  if (optional.isPresent()) {
      String value = optional.get();
      System.out.println(value);
  }
  ```

<br>

## 컬렉션(Collection)
- **컬렉션(Collection)**  
→ **데이터(객체)를 효율적으로 저장하고 관리**하기 위한 **자료구조**.  
→ 배열과 달리 크기가 동적으로 조정되며, 다양한 데이터 처리 기능을 제공.  

- **ArrayList**
  - `List` 인터페이스의 구현체, **배열 기반의 동적 리스트**
  - **인덱스를 기반**으로 한 빠른 조회(검색) 성능을 가짐.
  - **데이터 추가/삭제는 상대적으로 느림** (중간 삽입/삭제 시 요소를 이동해야 함).
  - 중복된 요소를 허용.

  ```java
  ArrayList<String> names = new ArrayList<>();

  // 데이터 추가
  names.add("kim");
  names.add("lee");
  names.add("park");
  names.add("hong");
  names.add("choi");

  // 순서 보장
  System.out.println("names = " + names);

  // 중복 허용
  names.add("park");
  System.out.println("names = " + names);

  // 단건 조회
  String name1 = names.get(0);
  System.out.println("name1 = " + name1);

  // 데이터 삭제
  names.remove("choi");
  System.out.println("names = " + names);
  ```

- **HashSet**
  - `Set` 인터페이스의 구현체, **중복을 허용하지 않는 집합**
  - 내부적으로 **HashMap을 사용**하여 요소를 저장.
  - **데이터 저장 순서를 보장하지 않음**.

  ```java
  HashSet<String> uniqueNames = new HashSet<>();

  // 데이터 추가 - 순서 보장하지 않음, 중복 불가
  uniqueNames.add("kim");
  uniqueNames.add("lee");
  uniqueNames.add("park");
  uniqueNames.add("hong");
  System.out.println("uniqueNames = " + uniqueNames);

  // get() 활용 불가
  // 중복 데이터 불가
  uniqueNames.add("lee");
  System.out.println("uniqueNames = " + uniqueNames);
  ```

- **HashMap**
  - `Map` 인터페이스의 구현체, **Key-Value(키-값) 쌍으로 데이터를 저장**.
  - `Key`는 **중복을 허용하지 않으며**, `Value`는 중복 가능.
  - 내부적으로 **해시 테이블을 사용하여 빠른 검색, 추가, 삭제 가능**.
  - **데이터 저장 순서를 보장하지 않음**.

  ```java
  // <키, 값> 저장
  HashMap<String, Integer> memberMap = new HashMap<>();

  // 데이터 추가 - 순서 보장되지 않음
  memberMap.put("kim", 17);
  memberMap.put("lee", 18);
  memberMap.put("park", 19);
  memberMap.put("hong", 20);
  System.out.println("memberMap = " + memberMap);

  // 키 중복 불가 - 값 덮어쓰기 발생
  memberMap.put("kim", 18);
  System.out.println("memberMap = " + memberMap);

  // 단건 조회
  Integer memberLee = memberMap.get("lee");
  System.out.println("memberLee = " + memberLee);

  // 데이터 삭제
  memberMap.remove("kim");
  System.out.println("memberMap = " + memberMap);

  // 키 확인
  Set<String> keySet = memberMap.keySet();
  System.out.println("keySet = " + keySet);

  // 값 확인
  Collection<Integer> values = memberMap.values();
  System.out.println("values = " + values);
  ```

<br>

## 제네릭(Generic)
- **클래스나 메서드에서 사용할 데이터 타입을 지정하지 않고, 나중에 인스턴스를 생성할 때 타입을 정할 수 있도록 하는 기능**

- 제네릭이 없는 경우
  - **재사용 불가** → 특정 타입을 고정적으로 사용해야 하므로, 다양한 타입을 지원하는 유연한 클래스를 만들 수 없음.

  ```java
  public class Box {

      private Integer item;

      public Box(Integer item) {
          this.item = item;
      }

      public Integer getItem() {
          return item;
      }
  }
  ```

  ```java
  public static void main(String[] args) {
      // 1. 재사용 불가
      Box box1 = new Box(100);
      // box1 = new Box("abc");
      // box1 = new Box(1.1);
  }
  ```

  - **낮은 타입 안정성** → **Object**를 사용하면 다양한 타입을 저장할 수 있지만, 형 변환이 필요하고 `ClassCastException` 발생 위험이 있음.

  ```java
  public class ObjectBox {

      private Object item;

      public ObjectBox(Object item) {
          this.item = item;
      }

      public Object getItem() {
          return item;
      }
  }
  ```

  ```java
  public static void main(String[] args) {
      // 2. 낮은 타입 안정성
      ObjectBox strBox = new ObjectBox("ABC");
      ObjectBox intBox = new ObjectBox(123);

      // item 활용을 위해 다운캐스팅 필요
      String strItem = (String) strBox.getItem();
      System.out.println("strItem = " + strItem);

      // 잘못된 다운캐스팅
      // String intItem = (String) intBox.getItem();
      // System.out.println("intItem = " + intItem);
  }
  ```

- **제네릭 클래스**
  - **재사용성** → 어떤 타입이든 유연하게 사용할 수 있음.
  - **타입 안정성 확보** → 컴파일 타임에 타입 체크가 가능하여 런타임 오류를 방지할 수 있으며, 형 변환이 필요 없음.
  - **타입 소거** → 파일 시점에 제네릭 타입 정보가 제거됨.

  ```java
  public class GenericBox<T> {

      private T item;

      public GenericBox(T item) {
          this.item = item;
      }

      public T getItem() {
          return this.item;
      }
  }
  ```

  ```java
  public static void main(String[] args) {
      // 1. 재사용성: 타입 소거 - T → Object
      GenericBox<String> strBox = new GenericBox<>("ABC");
      GenericBox<Integer> intBox = new GenericBox<>(123);
      GenericBox<Double> doubleBox = new GenericBox<>(1.1);

      // 2. 타입 안정성: 타입 소거 - 자동 다운캐스팅
      String strItem = strBox.getItem();
      Integer intItem = intBox.getItem();
      Double doubleItem = doubleBox.getItem();

      System.out.println("strItem = " + strItem);
      System.out.println("intItem = " + intItem);
      System.out.println("doubleItem = " + doubleItem);
  }
  ```

- **제네릭 메서드**  
→ 메서드 내부에서 사용할 타입을 선언하고, 호출 시점에 타입이 결정되는 메서드. 클래스의 제네릭 타입과 무관하게 동작할 수 있음.

  ```java
  public class GenericBox<T> {

      private T item;

      public GenericBox(T item) {
          this.item = item;
      }

      public T getItem() {
          return this.item;
      }

      // 일반 메서드
      public void printItem(T item) {
          System.out.print("printItem(T) = ");
          System.out.println(item);
      }

      // 제네릭 메서드
      public <S> void printBoxItem(S item) {
          System.out.print("printBoxItem(S) = ");
          System.out.println(item);
      }
  }
  ```

  ```java
  public static void main(String[] args) {
      // 1. 재사용성: 타입 소거 - T → Object
      GenericBox<String> strBox = new GenericBox<>("ABC");
      GenericBox<Integer> intBox = new GenericBox<>(123);
      GenericBox<Double> doubleBox = new GenericBox<>(1.1);

      // 2. 타입 안정성: 타입 소거 - 자동 다운캐스팅
      String strItem = strBox.getItem();
      Integer intItem = intBox.getItem();
      Double doubleItem = doubleBox.getItem();

      System.out.println("strItem = " + strItem);
      System.out.println("intItem = " + intItem);
      System.out.println("doubleItem = " + doubleItem);

      // 일반 메서드 - 클래스의 타입 매개변수 기준
      strBox.printItem("DEF");
      intBox.printItem(456);
      doubleBox.printItem(1.3);

      // 제네릭 메서드 - 독립적인 타입 매개변수
      strBox.printBoxItem("ABCDEF");
      strBox.printBoxItem(123456);
      strBox.printBoxItem(0.12345);
  }
  ```
  
- 클래스의 `<T>`는 클래스 전체에서 공통으로 사용되는 제네릭 타입.
- 메서드의 `<S>`는 클래스의 `<T>`와 무관하게 동작하는 독립적인 제네릭 타입.
- 제네릭 타입 매개변수 종류
  
| 표기 | 의미 | 사용 예시 |
|------|------|----------|
| **`T` (Type)** | 일반적인 타입 (Type) | `class Box<T> { T value; }` |
| **`E` (Element)** | 컬렉션 내부 요소 (Element) | `class List<E> {}` |
| **`K, V` (Key, Value)** | 키-값 쌍 (Map에서 사용) | `class Map<K, V> {}` |
| **`S, U`** | 추가적인 타입 매개변수 (Secondary, Specific) | `<S, U> void method(S s, U u);` |


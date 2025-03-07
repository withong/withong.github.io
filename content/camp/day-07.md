+++
title = "[내일배움캠프] 7일차. 객체지향"
date = "2025-02-25T21:00:00+09:00"
draft = "false"
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "java"]
ShowToc = true
+++

>  ✅ 문법 세션 1 - 프로그래밍 기초  
✅ Java 문법 강의 2주차  

<hr>

## Chapter 2. 객체지향 이해하기

<br>

### 클래스와 객체
- **클래스(class)** → 객체를 생성하기 위한 설계도. 속성과 기능을 정의하며, 동일한 구조를 가진 객체를 여러 개 생성할 수 있음.
- **객체(object)** → 클래스를 기반으로 생성된 실체. 메모리에 할당되어 동작하며, 개별적으로 속성과 기능을 가짐.
- **인스턴스화(instantiate)** → 클래스를 이용해 객체를 생성하는 과정. `new` 키워드를 사용해 객체를 메모리에 할당.
- **속성(property, field)** → 객체가 가지는 데이터. 클래스 내부에서 변수 형태로 선언되며, 객체마다 서로 다른 값을 가질 수 있음.
- **생성자(constructor)** → 객체가 생성될 때 호출되는 특수한 메서드. 속성을 초기화하고, 객체의 기본 상태를 설정하는 역할.
- **기능(method)** → 객체가 수행하는 동작. 클래스 내부에서 정의되며, 특정 입력을 받아 처리한 후 결과를 반환하거나 상태를 변경함.

<br>

### JVM 메모리 영역
- **Method Area** → 클래스 정보(클래스 이름, 메서드, 필드, static 변수 등)를 저장하는 공간. JVM이 시작될 때 로드되며, 모든 쓰레드가 공유.
- **Stack Area** → 메서드 호출 시 생성되는 스택 프레임을 저장하는 공간. 지역변수, 매개변수, 메서드 호출 정보가 포함되며, 호출이 끝나면 제거. 각 쓰레드마다 독립적인 스택을 가짐.
- **Heap Area** → 객체와 인스턴스 변수가 저장되는 공간. `new` 키워드로 생성된 객체가 저장되며, GC(Garbage Collector)가 관리. 모든 쓰레드가 공유.

<br>

### 래퍼클래스
- **래퍼클래스** → 기본형 데이터를 객체처럼 다룰 수 있도록 감싸는 클래스. `Integer`, `Double`, `Boolean` 등 기본형의 객체화된 버전.  
- **기본형 변수** → 실제 값을 직접 저장하는 변수. `int`, `double`, `char` 등 메모리에서 고정된 크기로 저장됨.  
- **참조형 변수** → 객체의 메모리 주소를 저장하는 변수. 배열, 클래스, 인터페이스 등 힙 영역에 저장된 객체를 가리킴.  
- **오토박싱 & 언박싱** → 기본형 ↔ 래퍼클래스 간 자동 변환 기능.  
  - **오토박싱**: 기본형 → 래퍼클래스로 자동 변환 (`int` → `Integer`)  
  - **언박싱**: 래퍼클래스 → 기본형으로 자동 변환 (`Integer` → `int`)
  
  ```java
  // 오토 박싱
  Integer num2 = 10; // 내부적으로 Integer.valueOf(10) 호출하여 객체 생성
  Integer num3 = Integer.valueOf(10); // 직접 Integer 객체 생성

  // 오토 언박싱
  int num4 = num3; // 내부적으로 num3.intValue()를 호출
  int num5 = num3.intValue(); // 직접 intValue() 호출하여 기본형으로 변환
  ```

<br>

### static
- **static 키워드** → 클래스 레벨에서 공유되는 변수 또는 메서드를 정의하는 키워드. `static`으로 선언된 변수와 메서드는 객체 생성 없이 사용 가능.
- **인스턴스 멤버 (인스턴스 변수, 인스턴스 메서드)** → 객체(인스턴스)마다 독립적으로 존재하는 변수와 메서드. `new`를 통해 객체를 생성해야 접근 가능.  
- **클래스 멤버 (클래스 변수, 클래스 메서드)** → `static` 키워드를 사용해 클래스에 소속된 변수와 메서드. 모든 객체가 공유하며, 객체 생성 없이 `클래스명.변수` 또는 `클래스명.메서드()` 형태로 접근 가능.

<br>

### final
- **final** → 변수, 메서드, 클래스에 사용되며 **값 변경 불가 또는 참조 변경 불가**를 의미.  
  - final 변수 → 값 변경 불가능 (초기화 후 재할당 불가)  
  - final 메서드 → 하위 클래스에서 오버라이딩 불가  
  - final 클래스 → 상속 불가  
- **상수 (`static final`)** → 변하지 않는 값을 저장하는 변수. `static final`로 선언하여 클래스 로드 시 한 번만 할당되며, 모든 인스턴스가 공유.
- **불변 객체 (Immutable Object)** → 내부 상태가 한 번 설정되면 절대 변경되지 않는 객체. `final` 키워드를 활용해 필드 변경을 막고, setter를 제공하지 않음. 

<br>

### 인터페이스
- **인터페이스란** → 클래스가 따라야 할 **설계 표준**을 정의하는 추상적 타입. 메서드의 시그니처(선언)만 포함하고, 구현은 하지 않음.
  ```java
  interface Animal {
      void makeSound(); // 메서드 선언 (구현 X)
  }
  ```

- **인터페이스를 사용하는 이유**  
  - **코드 표준화** → 여러 클래스가 동일한 방식으로 동작하도록 강제.
  - **결합도 낮추기** → 구현체 변경 시 영향 최소화.  
  - **다형성 지원** → 인터페이스 타입으로 여러 구현 객체를 다룰 수 있음.  

- **인터페이스 적용**  
    - **구현체 - 인터페이스를 구현한 클래스**
    - `Dog` 클래스는 `Animal` 인터페이스를 구현하며, **반드시 `makeSound()`를 오버라이딩해야 함**.
  ```java
  class Dog implements Animal {
      @Override
      public void makeSound() {
          System.out.println("멍멍!");
      }
  }
  ```

- **인터페이스의 다양한 기능**  
    - **인터페이스 다중 구현** 
      클래스는 여러 개의 인터페이스를 동시에 구현할 수 있음.
  ```java
  interface Flyable { void fly(); }
  interface Swimable { void swim(); }

  class Duck implements Flyable, Swimable {
      public void fly() { 
          System.out.println("오리가 난다!"); 
      }
      public void swim() { 
          System.out.println("오리가 수영한다!");
      }
  }
  ```

    - **인터페이스 다중상속**  
      인터페이스는 여러 개의 부모 인터페이스를 상속할 수 있음.
      
      ```java
      interface Animal { void eat(); }
      interface Pet { void play(); }

      interface Dog extends Animal, Pet { 
          void bark();
      }
      ```
- **인터페이스에 변수를 선언하는 경우**  
  인터페이스의 변수는 **항상 `public static final`(상수)**로 선언됨.  
  ```java
  interface Config {
      int MAX_USERS = 100; // public static final 자동 적용
  }
  ```

<br>

### 객체지향 - 캡슐화
- 객체의 **데이터(필드)와 메서드(기능)를 하나로 묶고, 외부에서 직접 접근을 제한하는 기법**. 중요한 데이터를 보호하고, 객체 내부의 구현을 숨길 수 있음.  

- **캡슐화가 필요한 이유**  
  - **데이터 보호** → 외부에서 직접 수정하지 못하게 막음.  
  - **코드 유지보수성 향상** → 내부 구현이 바뀌어도 외부에 영향을 주지 않음.  
  - **객체의 일관성 유지** → 잘못된 데이터 할당을 방지하고, 유효한 값만 저장 가능.  

- **접근제어자**  
  데이터와 메서드에 대한 접근을 제어하는 키워드.  
  - `public` → 어디서든 접근 가능  
  - `private` → 해당 클래스 내에서만 접근 가능  
  - `protected` → 같은 패키지 및 상속받은 클래스에서 접근 가능  
  - `default`(지정 안 함) → 같은 패키지 내에서만 접근 가능  

- **데이터 접근 - getter와 setter**  
  `private` 필드에 접근하기 위해 **getter(읽기), setter(쓰기)** 메서드를 제공.  
  ```java
  class Person {
      private String name;

      public String getName() { // Getter (읽기)
          return name;
      }

      public void setName(String name) { // Setter (쓰기)
          this.name = name;
      }
  }
  ```
  - `getName()`으로 이름을 읽고, `setName()`으로 값을 설정.  
  - **데이터 검증 로직 추가 가능** → 잘못된 데이터 입력 방지.  

<br>

### 객체지향 - 상속
- 기존 클래스를 확장하여 **코드 재사용성과 유지보수성을 높이는 개념**.  
    - `extends` 키워드를 사용하여 부모 클래스(슈퍼 클래스)의 속성과 메서드를 자식 클래스(서브 클래스)에서 상속받음.  

- **재사용성**  
  - 중복 코드 없이 **부모 클래스의 기능을 그대로 사용** 가능.  
  - 필요하면 **추가 기능을 확장하거나, 기존 기능을 재정의(오버라이딩)할 수 있음**.  
  ```java
  class Animal {
      void eat() { System.out.println("먹는다"); }
  }

  class Dog extends Animal {
      void bark() { System.out.println("멍멍"); }
  }

  Dog dog = new Dog();
  dog.eat();  // "먹는다" (부모 클래스 메서드 사용)
  dog.bark(); // "멍멍" (자식 클래스 메서드)
  ```

- **super - 부모 인스턴스**  
  - **`super` 키워드**를 사용해 부모 클래스의 멤버(변수, 메서드)에 접근 가능.  
  - **`super()`**는 부모 클래스의 생성자를 호출할 때 사용.  

  ```java
  class Animal {
      String name;
      
      Animal(String name) { this.name = name; }
  }

  class Dog extends Animal {
      Dog(String name) {
          super(name); // 부모 클래스(Animal)의 생성자 호출
      }
  }
  ```

- **확장 (Extending Functionality)**  
  - 자식 클래스는 부모 클래스의 기능을 **그대로 상속받고, 새로운 기능을 추가할 수 있음**.  
  ```java
  class Bird extends Animal {
      void fly() { System.out.println("날아간다"); }
  }

  Bird bird = new Bird();
  bird.eat(); // 부모 클래스 기능 사용
  bird.fly(); // 자식 클래스에서 확장한 기능
  ```

- **재정의 - 메서드 오버라이딩**  
  - 부모 클래스의 메서드를 자식 클래스에서 **동일한 이름으로 재정의(Override)**하여 동작 변경 가능.  
  - `@Override` 어노테이션을 사용해 의도적인 재정의를 명확히 표현.  
  ```java
  class Animal {
      void makeSound() { System.out.println("동물이 소리를 낸다"); }
  }

  class Dog extends Animal {
      @Override
      void makeSound() { System.out.println("멍멍"); } // 부모 메서드 재정의
  }

  Dog dog = new Dog();
  dog.makeSound(); // "멍멍" 출력 (오버라이딩된 메서드 실행)
  ```

- **추상 클래스 (Abstract Class)**  
  - **객체를 직접 생성할 수 없는 클래스**, 상속을 통해서만 사용 가능.  
  - 일부 메서드는 구현하지 않고 **추상 메서드(abstract method)**로 선언하여 자식 클래스에서 반드시 구현하도록 강제.  
  ```java
  abstract class Animal {
      abstract void makeSound(); // 추상 메서드 (구현 X)
  }

  class Dog extends Animal {
      @Override
      void makeSound() { System.out.println("멍멍"); }
  }
  ```

- **Object 클래스**  
  - **모든 클래스의 최상위 부모 클래스**  
  - `equals()`, `hashCode()`, `toString()`, `clone()` 등의 기본 메서드를 제공.  
  - 모든 Java 클래스는 `Object`를 자동으로 상속받음.  
  ```java
  class Example {}

  Example obj = new Example();
  System.out.println(obj.toString()); // Object의 메서드 사용
  ```

<br>

### 객체지향 - 추상화
- 객체의 **핵심적인 속성과 기능만 표현하고, 불필요한 세부 사항을 숨기는 개념**. 객체를 계층적으로 표현하여 공통된 개념을 뽑아내는 과정.
  ```java
  abstract class Vehicle {
      abstract void move(); // 핵심 기능만 선언
  }
  ```
- **인터페이스 상속을 활용한 추상 계층 표현**  
  - 인터페이스를 활용하면 **여러 계층에서 공통된 기능을 정의하고, 세부 구현은 각각 다르게 가능**.  
  - 다중 구현이 가능하여 유연한 설계를 지원.  
  ```java
  interface Animal {
      void makeSound(); // 추상화된 메서드
  }

  class Dog implements Animal {
      public void makeSound() {
          System.out.println("멍멍");
      }
  }

  class Cat implements Animal {
      public void makeSound() {
          System.out.println("야옹");
      }
  }
  ```
  - `Animal` 인터페이스를 상속받아 `Dog`, `Cat`이 각각의 방식으로 기능을 구현.  
  - **다형성을 활용할 수 있어 코드의 유연성이 증가**.  
- **클래스 상속을 활용한 추상 계층 표현**  
  - **추상 클래스**를 사용하여 공통된 속성과 메서드를 정의하고, 하위 클래스에서 이를 구체화.
  ```java
  abstract class Vehicle {
      String brand;

      abstract void move(); // 하위 클래스에서 구현

      void setBrand(String brand) { // 공통 메서드
          this.brand = brand;
      }
  }

  class Car extends Vehicle {
      @Override
      void move() {
          System.out.println("도로를 달린다");
      }
  }

  class Airplane extends Vehicle {
      @Override
      void move() {
          System.out.println("하늘을 난다");
      }
  }
  ```
  - **공통적인 속성을 유지하면서 확장 가능.**

<br>

### 객체지향 - 다형성
- 하나의 객체가 **여러 형태**를 가질 수 있는 성질. 부모 타입으로 자식 객체를 참조할 수 있어, 코드의 **유연성과 확장성**이 증가.  
  ```java
  class Animal {
      void makeSound() { System.out.println("동물이 소리를 낸다"); }
  }

  class Dog extends Animal {
      @Override
      void makeSound() { System.out.println("멍멍!"); }
  }

  Animal myPet = new Dog(); // 부모 타입으로 자식 객체 참조 (다형성)
  myPet.makeSound(); // "멍멍!" 출력
  ```

- **인터페이스를 활용한 다형성**  
  인터페이스 타입으로 여러 구현체를 다룰 수 있음.  
  ```java
  interface Animal {
      void makeSound();
  }

  class Dog implements Animal {
      public void makeSound() { System.out.println("멍멍!"); }
  }

  class Cat implements Animal {
      public void makeSound() { System.out.println("야옹!"); }
  }

  Animal pet1 = new Dog();
  Animal pet2 = new Cat();

  pet1.makeSound(); // "멍멍!"
  pet2.makeSound(); // "야옹!"
  ```
  - **같은 인터페이스를 구현한 객체라면, 동일한 방식으로 다룰 수 있음.**  
  - **추가 기능이 필요하면 새로운 클래스만 구현하면 됨 → 확장성 증가.**  
- **업캐스팅 (Upcasting)**  
  자식 객체를 **부모 타입**으로 변환 (자동 형변환).  
  ```java
  Dog dog = new Dog();
  Animal animal = dog; // 업캐스팅 (자동 변환)
  animal.makeSound(); // "멍멍!"
  ```
  - 부모 타입으로 변환되기 때문에 **부모가 가진 메서드만 호출 가능.**  
  - **하지만, 오버라이딩된 메서드는 그대로 실행됨.**  

- **업캐스팅 주의사항**  
  - 업캐스팅된 객체는 **자식 클래스의 고유 기능을 사용할 수 없음.**  
  ```java
  class Dog extends Animal {
      void bark() { System.out.println("짖는다!"); }
  }

  Animal pet = new Dog();
  pet.bark(); // 오류! Animal 타입에는 bark() 없음
  ```

- **다운캐스팅 (Downcasting)**  
  부모 타입을 **자식 타입**으로 변환 (명시적 형변환 필요).  
  ```java
  Animal pet = new Dog();
  Dog myDog = (Dog) pet; // 다운캐스팅 (명시적 변환)
  myDog.bark(); // "짖는다!"
  ```
  - 다운캐스팅하면 **자식 클래스의 고유 메서드를 사용할 수 있음.**  

- **다운캐스팅 주의사항**  
  - 잘못된 다운캐스팅 시 **`ClassCastException` 발생**.  
  ```java
  Animal pet = new Cat();
  Dog myDog = (Dog) pet; // 오류! Cat은 Dog로 변환할 수 없음.
  ```
  - 안전한 다운캐스팅을 위해 **`instanceof` 체크 필요.**  
  ```java
  if (pet instanceof Dog) {
      Dog myDog = (Dog) pet;
      myDog.bark();
  }
  ```

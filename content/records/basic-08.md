+++
title = "final"
date = "2025-02-25T09:00:00+09:00"
draft = false
topic = ["records"]
tag = ["Study", "Java"]
+++

## final -지역변수
- `final`이 붙은 **지역변수는 한 번만 값을 할당할 수 있음.**  
- 값이 할당된 이후에는 변경 불가능.  
- 메서드의 매개변수에도 `final`을 적용할 수 있으며, 이 경우 **해당 매개변수 값 변경이 불가능.**

```java
public class FinalLocalMain {
    public static void main(String[] args) {
        // final 지역변수1 - 선언 후 할당 (한 번만 가능)
        final int data1;
        data1 = 10; // 최초 할당 가능
        // data1 = 20; // 오류! final 변수는 한 번만 할당 가능

        // final 지역변수2 - 선언과 동시에 초기화
        final int data2 = 10;
        // data2 = 20; // 오류! 이미 값이 설정됨

        method(10);
    }

    // final 매개변수 (메서드 내부에서 변경 불가능)
    static void method(final int parameter) {
        // parameter = 20; // 오류! final 매개변수는 값 변경 불가
    }
}
```

- `final` 지역변수는 **최초 한 번만 값을 할당 가능**, 이후 변경 불가.
- `final` 매개변수는 **메서드 내부에서 값 변경이 불가능**.

<br>

## final - 필드(멤버변수)
클래스의 **멤버 변수(필드)에 `final`을 사용하면, 해당 필드는 한 번만 초기화 가능**함.

### (1) 생성자에서 초기화
- `final` 필드를 **생성자에서 한 번만 초기화 가능**.
- 이후에는 절대 변경할 수 없음.

```java
public class ConstructInit {
    final int value; // final 필드

    public ConstructInit(int value) { // 생성자에서 초기화
        this.value = value;
    }
}
```

- 객체 생성 시 생성자를 통해 `final` 필드 값을 설정.
- 객체별로 다른 값 설정 가능.
- 생성자에서 값을 설정한 이후에는 변경 불가.

<br>

### (2) 필드 선언과 동시에 초기화
- `final` 필드를 선언과 동시에 초기화하면, **모든 객체가 동일한 값을 가짐**.
- 생성자를 사용하지 않아도 됨.

```java
public class FieldInit {
    final int value = 10; // 필드 초기화
}
```

- 모든 객체가 동일한 값을 가짐.
- **불필요한 중복 생성 방지를 위해 `static`을 함께 사용하면 메모리 절약 가능.**

```java
public class FieldInit {
	// static final → 모든 객체가 공유하는 상수
    static final int CONST_VALUE = 10;
}
```

<br>

## 상수 (`static final`)
- `static final`을 사용하면 **클래스가 로드될 때 한 번만 초기화되고 모든 인스턴스에서 공유**됨.
- 일반적으로 **상수(Constant) 변수는 `public static final`로 선언하며, 변수명은 대문자로 작성**.

```java
public class Constant {
    public static final double PI = 3.14;
    public static final int MAX_USERS = 1000;
}
```

- `static` → 프로그램 실행 중 메모리에 한 번만 로드됨.
- `final` → 값 변경 불가.
- **중앙에서 한 번만 관리 가능하므로 유지보수가 쉬움**.

```java
System.out.println(Constant.PI);  // 3.14 출력
System.out.println(Constant.MAX_USERS);  // 1000 출력
```

- **매직 넘버(하드코딩된 숫자) 방지 → 의미를 이해하기 쉬움**  
- **코드 유지보수성이 증가 → 한 곳만 변경하면 전체 적용됨**

<br>

## final 변수와 참조 타입
- **기본형 변수(`int`, `double` 등)**: `final`을 붙이면 **값 자체가 변경 불가능**.  
- **참조형 변수(객체)**: `final`을 붙이면 **참조하는 객체(주소) 변경 불가능, 내부 값은 변경 가능**.

```java
public class FinalBasic {
    public static void main(String[] args) {
        final int num = 10;
        // num = 20; // 오류! final 변수는 값 변경 불가
    }
}
```

- `final`을 기본형 변수에 적용하면 **값 변경 불가**.

```java
class Data {
    public int value;
}

public class FinalRefMain {
    public static void main(String[] args) {
        final Data data = new Data();
        // data = new Data(); // 오류! 참조 대상 변경 불가

        // 참조 대상의 값은 변경 가능
        data.value = 10;
        System.out.println(data.value); // 10

        data.value = 20;
        System.out.println(data.value); // 20
    }
}
```

- `final`을 참조형 변수에 적용하면 **객체의 참조값(주소) 변경 불가능**.  
하지만, **객체 내부의 필드는 변경 가능**.

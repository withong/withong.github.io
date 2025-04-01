+++
title = "메모리 구조와 static"
date = "2025-02-14T21:00:00+09:00"
draft = false
topic = ["records"]
tag = ["Study", "Java"]
+++

## 메모리 구조  
Java 메모리는 크게 **메서드 영역, 스택 영역, 힙 영역**으로 나뉜다.

<br>

### 메서드 영역 (Method Area)
- **클래스 정보를 보관**하는 공간
- 프로그램을 실행하는 데 필요한 공통 데이터를 관리  
(클래스 정보, static 영역, 런타임 상수 풀 등)
- 프로그램 실행 중 변경되지 않는 데이터 저장  
- 프로그램이 종료될 때까지 유지됨

<br>

### 스택 영역 (Stack Area)
- 실제 프로그램이 실행되는 영역
- 메서드가 호출될 때마다 새로운 스택 프레임이 스택 영역에 생성됨
- 각 프레임은 **지역 변수, 매개변수, 연산 결과, 메서드 호출 정보** 저장함
- 메서드가 종료되면 해당 스택 프레임이 제거됨
- LIFO(Last In First Out) 방식으로 동작함

<br>

### 힙 영역 (Heap Area)
- 객체(인스턴스)가 생성되는 영역
- `new` 키워드로 생성된 객체와 배열이 저장됨
- 객체가 생성될 때마다 메모리 크기만큼 힙에서 메모리가 동적으로 할당됨
- 가비지 컬렉터에 의해 관리됨
- 참조되는 객체는 유지되며, 참조되지 않는 객체는 제거됨

<br>

## `static`
**`static` 변수와 `static` 메서드와 같은 클래스 레벨의 정보를 저장하는 공간.**  
`static` 키워드가 붙은 변수나 메서드는 인스턴스화된 객체와 관계없이 클래스 자체에 속하며, **메서드 영역의 `static` 영역에 저장됨**

<br>

### `static` 변수 (클래스 변수)
- **클래스가 로드될 때 한 번만 생성**되며, 모든 객체가 공유  
- 객체마다 개별적으로 존재하는 것이 아니라 **클래스 단위로 하나만 존재**  
- **클래스명.변수명** 형식으로 접근 가능 `객체 없이 사용 가능`

```java
package static1;

public class Data3 {
    public String name;
    public static int count;

    public Data3(String name) {
        this.name = name;
        count++;
    }
}
```
```java
package static1;

public class DataCountMain3 {

    public static void main(String[] args) {
        Data3 data1 = new Data3("A");
        System.out.println("A count = " + Data3.count);

        Data3 data2 = new Data3("B");
        System.out.println("B count = " + Data3.count);

        Data3 data3 = new Data3("C");
        System.out.println("C count = " + Data3.count);
    }
}
```
```
A count = 1
B count = 2
C count = 3
```

<br>

### `static` 메서드 (클래스 메서드)
- **객체를 생성하지 않고 클래스명으로 직접 호출 가능**  
- **static 변수만 직접 접근 가능 (인스턴스 변수 사용 불가)**  
- `this` 및 `super` 키워드를 사용할 수 없음 (객체에 의존하지 않음)

```java
package static2;

public class DecoData {

    private int instanceValue;
    private static int staticValue;

    public static void staticCall() {
        // instanceValue++; 인스턴스 변수 접근, 컴파일 오류
        // instanceMethod(); 인스턴스 메서드 접근, 컴파일 오류

        staticValue++; // 정적 변수 접근
        staticMethod(); // 정적 메서드 접근
    }

    public void instanceCall() {
        instanceValue++; // 인스턴스 변수 접근
        instanceMethod(); // 인스턴스 메서드 접근

        staticValue++; // 정적 변수 접근
        staticMethod(); // 정적 메서드 접근
    }

    private void instanceMethod() {
        System.out.println("instanceValue = " + instanceValue);
    }

    private static void staticMethod() {
        System.out.println("staticValue = " + staticValue);
    }
}
```
```java
package static2;

public class DecoDataMain {

    public static void main(String[] args) {
        System.out.println("1. 정적 호출");
        DecoData.staticCall();

        System.out.println("2. 인스턴스 호출1");
        DecoData data1 = new DecoData();
        data1.instanceCall();

        System.out.println("3. 인스턴스 호출2");
        DecoData data2 = new DecoData();
        data2.instanceCall();
    }
}
```
```
1. 정적 호출
staticValue = 1
2. 인스턴스 호출1
instanceValue = 1
staticValue = 2
3. 인스턴스 호출2
instanceValue = 1
staticValue = 3
```

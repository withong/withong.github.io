+++
title = "생성자"
date = "2025-02-10T21:00:00+09:00"
draft = false
topic = ["record"]
tag = ["Study", "Java"]
+++

## 생성자
객체가 생성될 때 자동으로 호출되는 메서드로, 객체를 초기화하는 데 사용됨.
생성자는 클래스와 동일한 이름을 가지며, 반환 타입이 없음.

<br>

### 기본 생성자 (Default Constructor)
클래스에 명시적으로 생성자가 없으면, Java가 자동으로 기본 생성자를 제공함.
기본 생성자는 매개변수가 없으며, 객체의 필드를 기본값으로 초기화 됨.

```java
public class Book {
    String title;
    String author;
    int page;
}
```
```java
Book book = new Book();
```

<br>

### 매개변수화된 생성자 (Parameterized Constructor)

생성자를 통해 객체 생성 시 값을 전달받아 초기화할 수 있음.
클래스에 명시적으로 생성자가 하나라도 있으면, 기본 생성자는 제공되지 않음.

```java
public class Book {
    String title;
    String author;
    int page;
    
    Book(String title, String author, int page) {
        this.title = title;
        this.author = author;
        this.page = page;
    }
}
```
```java
Book book1 = new Book(); // 오류
Book book2 = new Book("제목", "저자", 1);
```

<br>

### this
객체의 현재 인스턴스를 참조하는 키워드.
생성자에서 다른 생성자를 호출할 때 사용함. → **생성자 오버로딩**

```java
public class Book {
    String title;
    String author;
    int page;

    Book() {
        this("", "", 0);
    }

    Book(String title, String author) {
        this(title, author, 0);
    }

    Book(String title, String author, int page) {
        this.title = title;
        this.author = author;
        this.page = page;
    }
```
```java
Book book1 = new Book();
Book book2 = new Book("제목1", "저자1");
Book book3 = new Book("제목2", "저자2", 2);
```

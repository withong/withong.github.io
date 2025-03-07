+++
title = "[Java] 접근 제어자"
date = "2025-02-12T21:00:00+09:00"
draft = false
topic = ["java"]
tag = ["study", "java"]
ShowToc = true
ShowPostNavLinks = true
+++

## private
클래스 내부에서만 접근 가능  
> 클래스 내부의 속성과 기능을 숨기고, 직접적인 접근을 제한. 같은 클래스 내부에서만 접근 가능하며, 상속 관계에서도 자식 클래스가 접근할 수 없음.

## default
같은 패키지 내에서만 접근 가능, 명시적 키워드 없음  
> 패키지 내부에서만 사용할 기능을 숨기고, 외부 패키지에서는 접근을 막음. 접근 제한자를 명시하지 않으면 `default`가 적용되며, 같은 패키지에 있는 클래스만 접근 가능함.

## protected
같은 패키지 + 상속 관계에서 접근 가능
> 패키지 내에서는 자유롭게 접근할 수 있도록 하고, 다른 패키지에서는 상속받은 클래스만 접근 가능하도록 제한. 같은 패키지에서는 `default`와 동일하지만, 다른 패키지에서도 상속 관계라면 접근 가능.

## public
어디서든 접근 가능
> 어떤 클래스에서든 사용할 수 있도록 완전히 공개. 어디에서든 접근 가능하며, 패키지나 상속 관계와 무관하게 사용 가능.

<br>

| 접근 제어자  | 접근 가능 범위 | 상속 관계에서의 접근 |
|------------|----------------------|------------------|
| `private`  | 같은 클래스 내부에서만 가능 | ❌ (자식 클래스도 접근 불가) |
| `default`  | 같은 패키지 내에서만 가능 | ❌ (패키지가 다르면 접근 불가) |
| `protected` | 같은 패키지 + 상속받은 클래스에서만 가능 | ✅ (다른 패키지라도 상속받으면 접근 가능) |
| `public`   | 어디서든 접근 가능 | ✅ (제한 없음) |

<br>

```java
package access.a;

public class AccessData {

    public int publicField;
    int defaultField;
    private int privateField;

    public void publicMethod() {
        System.out.println("publicMethod 호출 " + publicField);
    }

    void defaultMethod() {
        System.out.println("defaultMethod 호출 " + defaultField);
    }

    private void privateMethod() {
        System.out.println("privateMethod 호출 " + privateField);
    }

    public void innerAccess() {
        System.out.println("내부 호출");
        publicField = 100;
        defaultField = 200;
        privateField = 300;
        publicMethod();
        defaultMethod();
        privateMethod();
    }

}
```

```java
package access.a; // 동일 패키지

public class AccessInnerMain {
    public static void main(String[] args) {
        AccessData data = new AccessData();
        // public 호출 가능
        data.publicField = 1;
        data.publicMethod();

        // 같은 패키지 default 호출 가능
        data.defaultField = 2;
        data.defaultMethod();

        // private 호출 불가
        // data.privateField = 3;
        // data.privateMethod();

        data.innerAccess();
    }
}
```

```java
package access.b; // 다른 패키지

import access.a.AccessData;

public class AccessOuterMain {
    public static void main(String[] args) {
        AccessData data = new AccessData();
        // public 호출 가능
        data.publicField = 1;
        data.publicMethod();

        // 다른 패키지 default 호출 불가
        // data.defaultField = 2;
        // data.defaultMethod();

        // private 호출 불가
        // data.privateField = 3;
        // data.privateMethod();

        data.innerAccess();
    }
}
```

<br>

## 클래스 레벨
클래스 레벨의 접근 제어자는 `public`, `default`만 사용할 수 있다.  
`public` 클래스는 반드시 파일명과 이름이 같아야 한다.

```java
package access.a;

public class PublicClass {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();
        DefaultClass1 class1 = new DefaultClass1();
        DefaultClass2 class2 = new DefaultClass2();
    }
}

class DefaultClass1 {

}

class DefaultClass2 {

}
```

```java
package access.a; // 동일 패키지

public class PublicClassInnerMain {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();
        DefaultClass1 class1 = new DefaultClass1();
        DefaultClass2 class2 = new DefaultClass2();
    }
}
```

```java
package access.b; // 다른 패키지

import access.a.PublicClass;

public class PublicClassOuterMain {
    public static void main(String[] args) {
        PublicClass publicClass = new PublicClass();

        // 다른 패키지 접근 불가
        // DefaultClass1 class1 = new DefaultClass1();
        // DefaultClass2 class2 = new DefaultClass2();
    }
}
```

<br>

## 캡슐화
✅ **데이터를 숨기고**(private) 외부에서 직접 접근하지 못하도록 제한한다.  
✅ 객체의 **내부 기능을 숨기고**(private, protected) 꼭 필요한 기능만 공개한다.
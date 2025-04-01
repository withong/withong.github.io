+++
title = "패키지"
date = "2025-02-11T21:00:00+09:00"
draft = false
topic = ["records"]
tag = ["Study", "Java"]
+++

## 패키지
클래스를 논리적으로 그룹화하는 방법.

![](https://velog.velcdn.com/images/ezro/post/e824c372-3653-4b2d-ae8e-51e3452a3a73/image.png)

### 패키지 사용
클래스 파일 맨 위에 package 키워드를 사용해 선언

```java
package pack;

public class Data {

    public Data() {
        System.out.println("패키지 pack Data 생성");
    }
}
```

같은 패키지 내 클래스 접근

```java
package pack;

public class PackageMain1 {

    public static void main(String[] args) {
        Data data = new Data();
    }
}
```

<br>

### import
다른 패키지의 클래스를 사용하기 위한 키워드.  
패키지 내 다른 클래스나 외부 라이브러리의 클래스를 참조할 수 있음.

```java
package pack.a;

public class User {

    public User() {
        System.out.println("패키지 pack.a 회원 생성");
    }
}
```

```java
package pack;

import pack.a.User;

public class PackageMain2 {

    public static void main(String[] args) {
        Data data = new Data();
        User user = new User();
    }
}
```

<br>

### 클래스 이름 중복
같은 이름의 클래스가 서로 다른 패키지에 존재하는 경우,  
둘 중 하나에 `패키지명.클래스명`을 명시적으로 사용하여 명확히 구분함.

```java
package pack.b;

public class User {

    public User() {
        System.out.println("패키지 pack.b 회원 생성");
    }
}
```

```java
package pack;

import pack.a.User;

public class PackageMain3 {

    public static void main(String[] args) {
        User userA = new User();
        pack.b.User userB = new pack.b.User();
    }
}
```

<br>

### 패키지 규칙
[필수] 패키지의 이름과 위치는 폴더(디렉토리) 위치와 같아야 한다.  
[관례] 패키지 이름은 모두 소문자를 사용한다.  
[관례] 패키지 이름의 앞 부분에는 회사의 도메인 이름을 거꾸로 사용한다.
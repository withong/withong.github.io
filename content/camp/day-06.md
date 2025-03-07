+++
title = "[내일배움캠프] 6일차. Java 기초"
date = "2025-02-24T21:00:00+09:00"
draft = "false"
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "java"]
ShowToc = "true"
+++

> ✅ Java 문법 강의 1주차  

<hr>

## Chapter 1. Java 기초 문법 다지기

<br>

### JDK (Java Development Kit)
- JDK → Java 프로그램을 개발할 때 필요한 개발 도구 모음
    - Javac(Java Compiler) → 자바코드를 바이트코드(.class)로 변환
    - JVM (Java Virtual Machine) → 바이트코드를 실행

<br>

### 컴퓨터의 기억 방식
- RAM (메모리) → 휘발성, 프로그램 실행 시 사용됨
- 보조기억장치 (HDD, SSD) → 영구 저장
- 비트(Bit) → 0 또는 1의 값
- 바이트(Byte) → 8비트로 구성

<br>

### 자바 프로젝트 관리
- 네이밍 규칙
    - 카멜케이스 (camelCase) → 변수, 메서드 (myVariableName)
    - 스네이크케이스 (snake_case) → (Java에서는 잘 사용하지 않음)
    - 클래스 이름 → PascalCase (MyClass)
    - 패키지 이름 → 소문자 (com.example.app)

<br>

### 변수와 자료형
- 변수 → 데이터를 저장하는 공간
- 자료형
    - 기본형: `int`, `double`, `char`, `boolean`
    - 참조형: `String`, `Array`, `Class`
```java
int num = 10;  // 정수형 변수
double pi = 3.14;  // 실수형 변수
boolean isJavaFun = true;  // 논리형 변수
```
- 형변환
    - 업캐스팅(작은 → 큰 자료형) → 자동 변환
    - 다운캐스팅(큰 → 작은 자료형) → 강제 변환
```java
double d = 10;  // 자동 형변환 (업캐스팅)
int i = (int) d;  // 강제 형변환 (다운캐스팅)
```

<br>

### 입출력 (I/O)
- 입력 (Scanner 사용)
```java
import java.util.Scanner;
Scanner scanner = new Scanner(System.in);
int num = scanner.nextInt();  // 정수 입력
```
- 출력 (System.out 사용)
```java
System.out.println("Hello Java!");  // 개행 포함 출력
System.out.print("No new line");  // 개행 없이 출력
System.out.println("\n줄 바꿈 포함");  // `\n` 사용
```

<br>

### 연산자
- 산술 연산자(`+`, `-`, `*`, `/`, `%`)
- 대입 연산자(`=`)
    - 복합 대입 연산자(`+=`, `-=`, `*=`, `/=` , `%=`)
- 증감 연산자(`++`, `--`)
    - 전위 연산 - 사용 전 연산
    - 후위 연산 - 사용 후 연산
- 비교 연산자(`==`, `!=`, `<`, `>`, `<=`, `>=`)
- 논리 연산자(`!`, `&&`, `||`)
- 문자열 비교(`equals()`)

<br>

### 조건문
```java
import java.util.Scanner;
Scanner scanner = new Scanner(System.in);

System.out.print("신호등 색상을 입력하세요(초록불, 노란불, 빨간불): ");
String light = scanner.nextLine();
```
```java
if (light.equals("초록불")) {
	System.out.println("건너세요!");
    
} else if (light.equals("노란불")) {
	System.out.println("주의하세요!");
    
} else if (light.equals("빨간불")) {
	System.out.println("멈추세요!");
    
} else {
	System.out.println("잘못된 입력입니다!");
}
```
```java
switch (light) {
	case "초록불":
    	System.out.println("건너세요!");
        break;
	case "노란불":
    	System.out.println("주의하세요!");
		break;
	case "빨간불":
    	System.out.println("멈추세요!");
        break;
	default:
    	System.out.println("잘못된 입력입니다!");
}
```

<br>

### 반복문
```java
for (int i = 0; i < 5; i++) {
    System.out.print(i + " ");  // 0 1 2 3 4
}
```
```java
int i = 0;
while (i < 5) {
    System.out.print(i + " ");
    i++;
}
```
```java
int i = 0;
do {
    System.out.print(i + " ");
    i++;
} while (i < 5);
```

<br>

### 배열
```java
// 1. 배열 선언
int[] arr1;

// 2. 배열 길이 할당
arr1 = new int[5];

// 3. 배열 선언과 길이 동시에 할당
int[] arr2 = new int[5];

// 4. 배열 선언과 동시에 배열의 요소 할당
int[] arr3 = {10, 20, 30, 40, 50};

// 배열의 길이
int arrLength = arr3.length;
System.out.println("arrLength = " + arrLength);

// 배열 요소 접근
System.out.println("배열의 1번째 요소 접근: " + arr3[0]);

// 배열 요소 변경
arr3[0] = 100;

// 향상된 for 문
for (int a : arr3) {
	System.out.println("a = " + a);
}

// 2차원 배열
int[][] arr4 = {
    {1, 2},
    {3, 4}
};
System.out.println(arr4[1][0]);  // 3
```

<br>

### 메서드
```java
public class Calculator {
	// 두 정수를 더하는 메서드 (반환값 있음: int)
    int sum(int value1, int value2) {
        int result = value1 + value2; // 두 값 더하기
        return result; // 결과 반환
    }

	// 반환값이 없는 메서드 (void)
    void printSum(int value1, int value2) {
        int result = value1 + value2;
        System.out.println("result = " + result);
    }

}
```
```java
public class Main {
    public static void main(String[] args) {
        Calculator calculator = new Calculator(); // Calculator 객체 생성

		// sum 메서드는 반환값이 있어서 변수에 저장
        int result = calculator.sum(1, 2);
        System.out.println("result = " + result);
        
        // printSum 메서드는 반환값이 없고 바로 출력됨
        calculator.printSum(3, 4);
    }
}
```
```
result = 3
result = 7
```
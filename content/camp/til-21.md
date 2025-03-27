+++
title = "[내일배움캠프] 3진법 뒤집기"
date = "2025-03-27T11:08:08+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "java", "algorism"]
+++

---

## 문제 설명
자연수 n이 매개변수로 주어집니다. n을 3진법 상에서 앞뒤로 뒤집은 후,  
이를 다시 10진법으로 표현한 수를 return 하도록 solution 함수를 완성해주세요.  

<br>

## 10진법 → k진법
1. n % k → 현재 자릿수 값 (맨 끝자리부터)
2. n = n / k → 다음 자릿수를 위해 나눔
3. 1~2를 반복하다가 n == 0이면 멈춤
4. 나머지들을 역순으로 조합

<br>

## k진법 → 10진법
```java
int result = Integer.parseInt("k진법으로 표현된 숫자", k);
```
해당 문자열을 k진법으로 해석한 후 10진법 정수로 반환

<br>

## 활용
```java
class Solution {
    public int solution(int n) {
        String str = "";

        while (n > 0) {
            str += n % 3;
            n /= 3;
        }

        return Integer.parseInt(str, 3);
    }
}
```

---
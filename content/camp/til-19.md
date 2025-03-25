+++
title = "[내일배움캠프] 최대 공약수와 최소 공배수"
date = "2025-03-25T20:07:52+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL", "java", "algorism"]
+++

---

## 문제 설명
두 수를 입력받아 두 수의 최대공약수와 최소공배수를 반환하는 함수, solution을 완성해 보세요. 배열의 맨 앞에 최대공약수, 그다음 최소공배수를 넣어 반환하면 됩니다. 예를 들어 두 수 3, 12의 최대공약수는 3, 최소공배수는 12이므로 solution(3, 12)는 [3, 12]를 반환해야 합니다.

## 코딩에서 쓰이는 공식
### 최대공약수(GCD): 유클리드 호제법 사용
GCD(a, b) = GCD(b, a % b)
나머지가 0이 될 때까지 반복하면, 그때의 값이 최대공약수

### 최소공배수(LCM):
LCM(a, b) = a * b / GCD(a, b)
곱해서 최대공약수로 나누면 최소공배수

```java
public class Solution {
    public int[] solution(int n, int m) {
        int gcd = getGCD(n, m);
        int lcm = n * m / gcd;
        return new int[]{gcd, lcm};
    }

    // 최대공약수 (유클리드 호제법)
    private int getGCD(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
}
```

## java.math.BigInteger 내장 메서드 활용
Java 8 이상 사용 가능

```java
import java.math.BigInteger;

public class Solution {
    public int[] solution(int n, int m) {
        BigInteger a = BigInteger.valueOf(n);
        BigInteger b = BigInteger.valueOf(m);

        // 최대공약수
        int gcd = a.gcd(b).intValue();

        // 최소공배수 = (a * b) / gcd
        int lcm = n * m / gcd;

        return new int[]{gcd, lcm};
    }
}
```

---
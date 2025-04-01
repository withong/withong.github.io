+++
title = "계산기 Lv 3 회고"
date = "2025-03-05T23:00:00+09:00"
draft = false
topic = ["TIL"]
tag = ["내일배움캠프", "TIL", "과제", "회고"]
+++

>✅ [문제] 알고리즘 & SQL 문제 풀기  
>✅ [과제] Java 계산기 만들기 level 3 마무리  
>✅ [과제] 과제 TIL & 트러블 슈팅 작성  

---

## ✅ 개인 과제 - 도전 구현
- [[내일배움캠프] 과제 - 계산기 3](https://velog.io/@ezro/camp-task-3) **/** [[GitHub] nbc-chapter-02/calculator/level3](https://github.com/withong/nbc-chapter-02/tree/main/calculator/level3)
<br>

## 🫠 `level 2 vs level 3`

<br>

### 개선된 점
- 단일 클래스 구조 → 다층 구조 (패키지 분리)
- Enum을 통한 연산자(Operator) 관리
- 제네릭(Operand) + BigDecimal 도입
- 람다와 스트림을 이용한 결과 필터링 기능 추가
- 커스텀 예외 클래스(CalculatorException) 활용

<br>

### 개선할 점
- `Operand<T>`가 BigDecimal로만 사용되는 점
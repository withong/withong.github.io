+++
title = "계산기 Lv 1-2 회고"
date = "2025-02-28T21:00:00+09:00"
draft = false
topic = ["records"]
tag = ["내일배움캠프", "TIL", "과제", "회고"]
+++

>✅ [과제] Java 계산기 만들기 level 2 수정  
>✅ [과제] 과제 TIL & 트러블 슈팅 작성  
>✅ [문제] 알고리즘 문제 풀기  

<hr>
<br>

## ✅ 개인 과제 - 필수 구현
- [[내일배움캠프] 과제 - 계산기 1](https://velog.io/@ezro/camp-task-1) **/** [[GitHub] nbc-chapter-02/calculator/level1](https://github.com/withong/nbc-chapter-02/tree/main/calculator/level1)
- [[내일배움캠프] 과제 - 계산기 2](https://velog.io/@ezro/camp-task-2) **/** [[GitHub] nbc-chapter-02/calculator/level2](https://github.com/withong/nbc-chapter-02/tree/main/calculator/level2)
<br>

## 🫠 `level 1 vs level 2`

### 개선된 점
- 계산 결과 객체, 계산기 기능 클래스, 프로그램 실행으로 역할 분리
- `calculate()`의 반환 타입 `double`에서 `String`으로 변경  
→ 연산 오류 처리 반환값 `-9999`에서 `의미 있는 메시지`로 개선
- `CalculationRecord` 클래스 추가하여 연산 기록 저장 방식 개선
- `trimRecords()` 추가하여 과도한 데이터 누적 방지
- 0으로 나누기 처리 `Error`에서 `정의되지 않음`으로 개선
- int 범위 초과 처리 `Error`에서 `연산 범위 초과`로 개선
- 최근 연산 기록 조회할 수 있도록 `showRecords()` 추가
- 사용자가 직접 삭제할 수 있도록 `removeRecords()` 메서드 추가.
<br>

### 개선할 점
- `int` 범위 초과하는 입력값, 연산 결과 오류 → `long`으로 확장
- 오류 처리 방식 `handleError` → 커스텀 예외 처리

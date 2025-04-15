+++
title = "뉴스피드 API 트러블 슈팅"
date = "2025-04-15T17:53:03+09:00"
draft = false
tag = ["내일배움캠프", "과제", "뉴스피드", "트러블 슈팅"]
+++

<hr>
<br>

## @JsonFormat을 이용한 사용자 요청 값 검증

<br>

### 상황

회원가입 시 사용자의 생년월일을 `UserCreateRequestDto`에서 `LocalDate` 타입으로 받고 있으며, 아래와 같이 `@JsonFormat`을 사용하여 날짜 형식을 검증하고자 했음.

```java
// 생년월일
@NotNull(message = "생년월일은 필수 입력값입니다.")
@JsonFormat(pattern = "yyyyMMdd")
private final LocalDate birth;
```

<br>

### 문제

`@JsonFormat`은 지정한 형식과 다른 값이 입력될 경우, 컨트롤러에 진입하기 전에 JSON → Java 객체로 변환하는 Jackson 역직렬화 과정에서 예외를 발생시킴. 이때 발생하는 예외는 `@Valid`에서 처리되지 않고 `BindingResult`이나 `MethodArgumentNotValidException`으로도 잡을 수 없으며, `HttpMessageNotReadableException`으로 처리되어 클라이언트에게는 단순한 400 Bad Request 응답으로 전달됨.

<br>

### 원인

- `@JsonFormat`은 Jackson의 JSON 직렬화 및 역직렬화 시점에서 적용할 형식을 지정하는 용도임.
- 유효성 검증을 위한 어노테이션이 아니며, Spring의 Bean Validation과는 무관하게 발생함.

<br>

### 분석

- 사용자 입력 값의 형식 검증 용도로 `@JsonFormat`을 사용하는 것은 적절하지 않음.
- 대안으로 생년월일을 `String`으로 받고 `@Pattern`을 통해 원하는 형식을 검증한 후 `LocalDate`로 변환하는 방법이 있으나, 별도의 변환 단계가 추가되며 Entity와의 일관성이 떨어짐.

<br>

### 해결
불필요한 구조 변경 없이, 입력 오류에 대한 구체적인 피드백을 제공하는 방향으로 개선함.
- 기존: 유효성 검증 예외 메시지 형식과 동일하게 "올바른 날짜 형식이 아닙니다." 전달
- 개선: JSON → Java type 변환 과정에서 발생한 예외임을 명확히 전달

```java
    @ExceptionHandler(HttpMessageNotReadableException.class)
    protected ResponseEntity<ExceptionResponseDto> handleJsonFormatException(HttpMessageNotReadableException e) {
        Throwable cause = e.getCause();

        // 날짜 파싱 실패인지 확인
        if (cause instanceof InvalidFormatException invalidFormatEx) {
            String targetField = extractField(invalidFormatEx);
            String targetValue = invalidFormatEx.getValue().toString();
            String expectedType = invalidFormatEx.getTargetType().getSimpleName();

            String message = String.format("[%s] 필드는 '%s' 값을 '%s' 타입으로 변환할 수 없습니다.",
                    targetField, targetValue, expectedType);

            ExceptionCode code = ExceptionCode.INVALID_DATE_FORMAT;

            return ResponseEntity.status(code.getStatus().value())
                    .body(ExceptionResponseDto.builder()
                            .status(code.getStatus().value())
                            .code(code.getCode())
                            .message(message)
                            .build());
        }

        return ExceptionResponseDto.dtoResponseEntity(ExceptionCode.VALIDATION_FAILED);
    }

    private String extractField(InvalidFormatException ex) {
        return ex.getPath().stream()
                .findFirst()
                .map(JsonMappingException.Reference::getFieldName)
                .orElse("알 수 없음");
    }
```

<br>
<hr>
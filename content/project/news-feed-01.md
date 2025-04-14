+++
title = "뉴스피드 API 트러블 슈팅"
date = "2025-04-03T22:28:58+09:00"
draft = true
tag = ["내일배움캠프", "과제", "뉴스피드", "트러블 슈팅"]
+++

<hr>
<br>

## @JsonFormat을 이용한 생년월일 검증의 한계

### 상황

`UserCreateRequestDto`에서 사용자의 생년월일을 `LocalDate`로 받고 있으며, 다음과 같이 `@JsonFormat`을 지정해 날짜 형식을 검증하고자 했음.

```java
// 생년월일
@NotNull(message = "생년월일은 필수 입력값입니다.")
@JsonFormat(pattern = "yyyyMMdd")
private final LocalDate birth;
```

### 문제

- `@JsonFormat`은 클라이언트에서 `"birth": "1990-01-01"`처럼 지정한 패턴과 일치하지 않는 값이 전달되면 예외를 발생시킴.
- 그러나 이때 발생하는 예외는 `@Valid` 검증과 무관한 **JSON → Java 객체 변환 과정**에서의 예외로, `HttpMessageNotReadableException`이 발생함.
- 해당 예외는 컨트롤러 진입 이전에 발생하므로, 일반적인 검증 흐름에서 사용하는 `BindingResult`나 `MethodArgumentNotValidException`으로는 처리할 수 없음.
- 예외가 발생한 필드나 원인을 추적하려면 추가적인 작업이 필요하며, 그 자체로 진입 장벽이 생김.

### 원인

- `@JsonFormat`은 Jackson의 직렬화/역직렬화 시점에서 사용되는 포맷 지정용 어노테이션임.
- 사용자의 입력값을 유효성 검증하기 위한 용도로 만들어진 어노테이션이 아니며, 형식이 맞지 않는 경우 `InvalidFormatException`을 유발하게 됨.
- 이 예외는 스프링의 검증기가 개입할 수 없는 시점에서 발생하며, 개발자가 직접 핸들링하지 않으면 클라이언트에게는 단순한 400 Bad Request로 보이게 됨.

### 분석

- 사용자 입력값의 형식 검증 용도로 `@JsonFormat`을 사용하는 것은 적절하지 않음.
- 형식 오류가 Jackson에서 발생하므로, 이를 적절히 가공해 응답하지 않으면 사용자에게 구체적인 피드백을 주기 어려움.
- 대안으로는 DTO에서 `String` 타입으로 받고 `@Pattern` 등을 통해 명시적으로 검증하는 방법이 있지만, 이후 다시 `LocalDate`로 변환해야 한다는 점이 불편함.

### 해결

- 리팩토링을 통해 `String` 타입 + `@Pattern` 방식으로 전환하는 방법도 고려했으나, 이후 `LocalDate`로의 수동 변환이 번거롭고 불필요한 로직이 추가된다는 점에서 제외함.
- 대신, JSON 변환 과정에서 발생한 예외를 명확하게 전달할 수 있도록 `HttpMessageNotReadableException`을 처리하는 글로벌 예외 핸들러를 작성하여 해결.

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

- 위와 같은 방식으로, 클라이언트가 전달한 날짜 형식이 잘못되었을 때 그 필드명, 값, 기대하는 타입을 포함한 상세 메시지를 응답으로 전달할 수 있음.
- 불필요한 구조 변경 없이 현재 구조를 유지하면서도, 입력 오류에 대해 구체적인 피드백을 제공할 수 있도록 마무리함.

### 결론

- `@JsonFormat`은 입력 검증용 어노테이션으로는 적절하지 않으나, JSON 파싱 과정의 예외를 글로벌 핸들러에서 적절히 가공하면 충분히 사용할 수 있음.
- 입력값의 형식 오류에 대해 사용자 친화적인 응답을 제공하고자 한다면, 예외 핸들링 전략을 통해 해결하는 접근도 유효한 방법임.

<br>
<hr>
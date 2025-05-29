+++
title = "기차 예매 API 트러블 슈팅"
date = "2025-05-23T23:18:16+09:00"
draft = false
tag = ["내일배움캠프", "팀 프로젝트", "트러블 슈팅"]
+++

---
## GitHub
### ▶ [five-lock-run](https://github.com/team-five-lock-run/five-lock-run.git)

<hr>
<br>

## Redis 목록 조회 성능 최적화 (KEYS vs SCAN)

### 기존 방식: `KEYS` 명령어 사용

처음에는 Redis에 저장된 예약 정보를 사용자 ID 기준으로 조회할 때 `KEYS` 명령어를 사용했다.

```java
public List<Reservation> getReservationsByUser(Long userId) {
    Set<String> keys = redisTemplate.keys("user-reservation:" + userId + ":*");
    if (keys == null || keys.isEmpty()) return Collections.emptyList();

    List<Object> values = redisTemplate.opsForValue().multiGet(keys);
    if (values == null) return Collections.emptyList();

    return values.stream()
            .filter(Objects::nonNull)
            .map(obj -> (Reservation) obj)
            .collect(Collectors.toList());
}
```

이 코드는 간단하고 동작도 잘 되지만, 운영 환경에서는 다음과 같은 문제가 발생할 수 있다.

* `redisTemplate.keys()`는 Redis의 `KEYS` 명령어를 사용한다.
* 이 명령어는 Redis에 저장된 모든 키를 **한 번에** 뒤져서 조건에 맞는 키를 찾아낸다.
* 데이터가 수천 개, 수만 개 이상 쌓이면 이 조회 자체가 Redis 서버에 큰 부담이 된다.
* 특히 Redis는 기본적으로 하나의 명령어를 처리하는 동안 다른 명령어를 처리하지 못하는 구조라, `KEYS` 명령이 실행 중이면 다른 사용자 요청이 지연되거나 막힐 수 있다.
* 실제 운영 서비스에서는 이로 인해 응답 속도가 느려지거나 서버가 잠깐 멈춘 것처럼 보일 수 있다.

<br>

### 개선 방식: `SCAN` 명령어 사용

이 문제를 해결하기 위해, Redis가 키를 **조금씩 나눠서** 찾아오는 방식인 `SCAN` 명령어로 변경했다.

```java
private List<ReservationResponse> getReservations(String pattern) {
    List<ReservationResponse> reservationResponses = new ArrayList<>();

    ScanOptions options = ScanOptions.scanOptions()
            .match(pattern)
            .count(100)
            .build();

    log.info("예약 정보 조회 중...");
    Cursor<byte[]> cursor = redisTemplate.getConnectionFactory().getConnection().scan(options);

    try {
        while (cursor.hasNext()) {
            byte[] key = cursor.next();
            String keyString = new String(key);
            Object value = redisTemplate.opsForValue().get(keyString);

            if (value instanceof Reservation reservation) {
                log.info("예약 정보 조회 성공: " + keyString);
                ReservationResponse reservationResponse = ReservationResponse.from(keyString, reservation);
                reservationResponses.add(reservationResponse);
            } else {
                log.info("예약 정보 조회 값 null: " + keyString);
            }
        }
    } finally {
        cursor.close();
    }

    return reservationResponses;
}
```

변경된 코드는 `SCAN` 명령어를 사용해서 다음과 같이 개선되었다.

* Redis 전체를 한꺼번에 뒤지지 않고, 조금씩 나눠서 필요한 키만 점진적으로 조회함
* `MATCH` 옵션으로 필요한 키 형식(`user-reservation:{userId}:*`)만 찾도록 설정함
* `COUNT` 옵션으로 한 번에 가져올 키 수를 제한해서 서버 부하를 줄임
* `Cursor`를 통해 여러 번 반복하면서 전체 키를 하나하나 확인해 나감

<br>

### 결과

* Redis 서버의 응답이 안정적으로 유지됨
* 데이터가 많아져도 서비스 속도가 느려지거나 멈추는 현상이 사라짐
* 동시에 여러 사용자가 접근해도 전체 시스템에 영향 없이 조회 가능
* Redis 운영 환경에서 권장되는 방식으로 전환하여 서비스 안정성 확보

<br>
<hr>
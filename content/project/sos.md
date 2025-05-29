+++
title = "배달 서비스 API 트러블 슈팅"
date = "2025-04-28T23:09:11+09:00"
draft = false
tag = ["내일배움캠프", "팀 프로젝트", "트러블 슈팅"]
+++

---
## GitHub
### ▶ [project-sos](https://github.com/nine-trillion/project-sos.git)

<hr>
<br>

## Service 간 주입 방식의 코드 구현

### 1. 문제의 시작: `findById()` 예외 처리는 어디에서 해야 하는가?

Repository에서 `findById()`를 호출했을 때 조회 결과가 없는 경우, 예외를 **Repository에서 처리할지**, **Service에 위임하여 처리할지** 결정해야 한다. 두 방식은 아래와 같이 나뉜다.

#### (1) Repository에서 예외를 처리하는 방식

```java
public interface UserRepository extends JpaRepository<User, Long> {

    default User findByIdOrThrow(Long id) {
        return findById(id)
                .orElseThrow(() -> new EntityNotFoundException("User not found with id: " + id));
    }
}
```

```java
public User findUser(Long id) {
    return userRepository.findByIdOrThrow(id);
}
```

#### (2) Service에서 예외를 처리하는 방식

```java
public User findUser(Long id) {
    return userRepository.findById(id)
            .orElseThrow(() -> new EntityNotFoundException("User not found with id: " + id));
}
```

### 2. 분석: Repository가 아닌 Service에서 예외 처리를 해야 하는 이유

* **역할 분리 원칙**에 따라 Repository는 단순히 데이터 접근만을 담당하고, 예외 처리나 비즈니스 로직은 Service 계층에서 관리하는 것이 바람직하다.
* 예외 처리 방식 변경 시, Service에서 처리하면 수정 범위를 Service에 국한시킬 수 있어 **유지보수성이 향상**된다.
* 예외 메시지나 예외 타입을 도메인에 따라 다르게 정의할 수 있어 **비즈니스 규칙을 명확히 표현**할 수 있다.
* 검증 책임을 Service로 일관되게 위임하면 **다른 Service에서의 재사용과 일관성 유지가 수월**하다.

이러한 이유로, 예외 처리는 Repository보다는 Service 계층에서 수행하는 것이 더 적절하다.

### 3. 확장된 문제 1: Service에서 예외 처리한 결과를 다른 Service에서 가져다 써야 하는가?

Service에서 검증까지 포함된 메서드를 제공하고, 다른 Service는 해당 메서드를 호출해 필요한 엔티티를 가져오는 방식이 더 일관되고 안전하다.

#### 예시

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public User findUserById(Long id) {
        return userRepository.findById(id)
                .orElseThrow(() -> new EntityNotFoundException("User not found with id: " + id));
    }
}
```

```java
@Service
public class OrderService {
    private final UserService userService;

    public OrderService(UserService userService) {
        this.userService = userService;
    }

    public void placeOrder(Long userId) {
        User user = userService.findUserById(userId);
        // 주문 생성 로직
    }
}
```

이런 구조에서는 **중복 예외 처리 방지**, **도메인 별 책임 분리**, **비즈니스 로직의 재사용성 향상** 같은 장점이 있다.

### 4. 확장된 문제 2: AService에서 BRepository를 직접 주입받아도 괜찮은가?

AService가 BRepository를 직접 사용하는 방식은 다음과 같은 문제를 일으킬 수 있다.

* **Service 계층의 도메인 경계 침범**: 다른 도메인의 내부 구현에 접근하게 되어 구조가 엉킬 수 있다.
* **높은 결합도**: BRepository의 변경이 AService에 직접 영향을 주게 되며, 테스트와 유지보수가 어려워진다.
* **역할 혼란**: AService가 B도메인의 검증 책임까지 떠맡게 되어 설계 원칙이 무너질 수 있다.

따라서, BRepository에 직접 접근하는 대신 BService를 주입받아 필요한 기능을 위임하는 방식이 더 안정적이다.

#### 개선된 구조 예시

```java
public interface StoreService {
    Store findStoreById(Long id);
}

@Service
public class StoreServiceImpl implements StoreService {
    private final StoreRepository storeRepository;

    public StoreServiceImpl(StoreRepository storeRepository) {
        this.storeRepository = storeRepository;
    }

    @Override
    public Store findStoreById(Long id) {
        return storeRepository.findById(id)
                .orElseThrow(() -> new EntityNotFoundException("Store not found with id: " + id));
    }
}
```

```java
@Service
public class OrderService {
    private final StoreService storeService;

    public OrderService(StoreService storeService) {
        this.storeService = storeService;
    }

    public void placeOrder(Long storeId) {
        Store store = storeService.findStoreById(storeId);
        // 주문 생성 로직
    }
}
```

```java
@Component
@RequiredArgsConstructor
public class StoreValidator {
    private final StoreRepository storeRepository;

    public void validateOwner(Long userId, Long storeId) {
        Store store = storeRepository.findById(storeId)
                .orElseThrow(() -> new StoreException(StoreError.NOT_FOUND_STORE));

        if (!store.getOwner().getId().equals(userId)) {
            throw new StoreException(StoreError.UNAUTHORIZED_STORE_OWNER);
        }
    }
}
```

별도의 Validator 컴포넌트를 통해 도메인 단위의 세부 검증을 담당하게 하면, 핵심 Service는 핵심 비즈니스 로직에 집중할 수 있다.

### 5. 결론: Service 간 주입 방식으로 구현

* Repository는 단순한 데이터 접근만을 책임지고, 예외 처리는 Service에서 수행한다.
* 검증 로직은 Service 또는 별도 Validator에서 수행하며, 이를 다른 Service에서 재사용한다.
* AService가 B 도메인의 데이터를 사용할 때는 BRepository를 직접 접근하기보다 BService를 통해 간접적으로 접근한다.
* Service 간 주입은 **단방향으로**, **필요한 경우에만**, **인터페이스를 통해 결합도를 낮춘 형태**로 최소화한다.
* 지나친 Service 간 의존성은 시스템 복잡도를 증가시키므로, 검증 책임과 로직 배치를 신중히 판단해야 한다.

이러한 방식은 코드 구조를 깔끔하게 유지하고, 역할을 분리하면서도 비즈니스 흐름을 일관성 있게 관리하는 데 효과적이다. 이번 SOS 프로젝트에서는 이러한 기준에 따라 Service 간 주입과 예외 처리 방식을 적용하기로 했다.

<br>
<hr>
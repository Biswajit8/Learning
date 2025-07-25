**Optional use**

```java
public class BillRepositoryImpl implements BillRepository {
  // ...
  public Optional<Bill> findById(long billId) {
      return Optional.ofNullable(billMap.get(billId));
  }
}

public class PaymentServiceImpl implements PaymentService {
  private BillRepository billRepository;

  public PaymentServiceImpl(BillRepository billRepository) {
      this.billRepository = billRepository;
  }

  @Override
  public Payment makePayment(long billId) throws InvalidBillException {
      Bill bill = billRepository.findById(billId).orElseThrow(() -> new InvalidBillException("Bill not found"));
      // ...
  }
}
```

----

**Circuit Breaker with Retry mechanism implementation**

References:

https://www.youtube.com/watch?v=RZFJLeHf5kw

https://mvnrepository.com/artifact/io.github.resilience4j/resilience4j-circuitbreaker

https://resilience4j.readme.io/docs/circuitbreaker

https://resilience4j.readme.io/docs/retry

https://resilience4j.readme.io/docs/getting-started-3

**Steps:**

1. Add the following dependencies to pom.xml

```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
</dependency>
<dependency>
  <groupId>io.github.resilience4j</groupId>
  <artifactId>resilience4j-spring-boot2</artifactId>
  <version>1.5.0</version>
</dependency>
```

2. Add the following annotations to the method

```
@CircuitBreaker(name = "userServiceCircuitBreaker", fallbackMethod = "fallbackMethod")
@Retry(name = "userServiceRetry")
public String ...
```

3. Create a new fallback method which takes 'Exception' as parameter

Note: Return type of fallback method should be same as return type of original method.

```java
public String fallbackMethodCMS(Exception e) {
  log.warn("XYZ API is not responding. Exception: " + e.getMessage());
  return "fallback";
}
```

4. Add the following properties to application.yml

```
---
management:
  health:
    circuitbreakers:
      enabled: true
  endpoints:
    web:
      exposure:
        include: health
  endpoint:
    health:
      show-details: always


resilience4j:
  circuitbreaker:
    instances:
      executionServiceCircuitBreaker:
        register-health-indicator: true
        event-consumer-buffer-size: 10
        failure-rate-threshold: 50
        minimum-number-of-calls: 5
        automatic-transition-from-open-to-half-open-enabled: true
        wait-duration-in-open-state: 1m
        permitted-number-of-calls-in-half-open-state: 3
        sliding-window-size: 5
        sliding-window-type: COUNT_BASED
    circuit-breaker-aspect-order: 1

  retry:
    instances:
      executionServiceRetry:
        max-retry-attempts: 2
        wait-duration: 50ms
    retry-aspect-order: 2
```

Circuit breaker gets a higher priority over retry mechanism by default, so we set priority using circuit-breaker-aspect-order & retry-aspect-order.

3 or more calls out of last 5 calls fail -> change from CLOSED to OPEN state

wait for 1 min in OPEN state (execute fallback logic till then, NO retry) then transition automatically to HALF-OPEN state

in HALF-OPEN state, 2 or more calls out of 3 calls fail -> change from HALF-OPEN to OPEN again, otherwise change to CLOSED

5. Testing

http://localhost:8080/actuator/health

---

**Spring Boot 3.1.0 New Feature | Docker Compose Module**

https://www.youtube.com/watch?v=-7b_jG9RY78

Automatically spin up & destroy docker containers using `docker-compose.yml` without any additional configuration.

https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.1.0-RC1-Release-Notes

<img width="1057" alt="image" src="https://github.com/user-attachments/assets/a6abbdc3-3a47-484a-9b7c-1928b40bdcbb" />

Add the following dependency in `pom.xml`

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-docker-compose</artifactId>
</dependency>
```

---

**@EmbeddedId annotation**

@EmbeddedId is employed to denote a composite primary key that comprises multiple columns, useful in scenarios where a single column does not uniquely identify a record.

https://medium.com/jpa-java-persistence-api-guide/hibernate-composite-primary-keys-with-embeddedid-annotation-46a8171e8d22

---

**@Retryable Annotation for Automatic Retries**

Spring’s @Retryable annotation allows methods to automatically retry when they fail due to short-lived problems.

https://medium.com/@AlexanderObregon/using-springs-retryable-annotation-for-automatic-retries-c1d197bc199f

---

**Spring Debugger**

https://www.youtube.com/watch?v=t34TEVx5YuM

https://blog.jetbrains.com/idea/2025/06/demystifying-spring-boot-with-spring-debugger/

- Bean Discovery: Bean Visualization
- Properties Resolution: Runtime Properties Loading
- Runtime Evaluator: Spring Expression Evaluator
- Conditional Logging Breakpoint 
- Auto Connect Database: Auto-detection of Docker Compose Databases
- Transaction Inspection: Runtime Transaction Context

---


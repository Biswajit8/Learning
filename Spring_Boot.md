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

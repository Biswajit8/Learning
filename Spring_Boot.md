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


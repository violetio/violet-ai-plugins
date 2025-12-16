# Testing Fundamentals

## Testing Pyramid

```
        /\
       /  \      E2E Tests (Few, Slow, Expensive)
      /----\
     /      \    Integration Tests (Some)
    /--------\
   /          \  Unit Tests (Many, Fast, Cheap)
  /------------\
```

### Unit Tests (70%)
- Test individual functions/methods
- Fast execution (< 100ms each)
- No external dependencies (mocked)
- High coverage of business logic

### Integration Tests (20%)
- Test component interactions
- Database, API, message queue integration
- Slower but realistic
- Focus on boundaries

### E2E Tests (10%)
- Test complete user flows
- Browser/API level
- Slowest, most expensive
- Critical paths only

## Test Structure (AAA Pattern)

```java
@Test
void shouldCalculateTotalWithDiscount() {
    // Arrange - Set up test data
    Order order = new Order();
    order.addItem(new Item("SKU-1", 100.00));
    order.setDiscountPercent(10);

    // Act - Execute the code
    double total = order.calculateTotal();

    // Assert - Verify results
    assertThat(total).isEqualTo(90.00);
}
```

## Naming Conventions

```java
// Pattern: should_[expectedBehavior]_when_[condition]
void shouldReturnEmptyList_whenNoOrdersExist() { }
void shouldThrowException_whenOrderIdIsNull() { }
void shouldCalculateCorrectTotal_whenDiscountApplied() { }
```

## Mocking Patterns

### Mock External Dependencies
```java
@Mock
private OrderRepository orderRepository;

@Mock
private PaymentService paymentService;

@InjectMocks
private OrderService orderService;

@Test
void shouldProcessOrder() {
    when(orderRepository.findById(1L))
        .thenReturn(Optional.of(testOrder));
    when(paymentService.charge(any()))
        .thenReturn(PaymentResult.success());

    orderService.processOrder(1L);

    verify(paymentService).charge(any());
}
```

### Don't Mock What You Own
- Mock external services (payment, email)
- Don't mock your own domain objects
- Don't mock simple value objects

## Test Data Patterns

### Builder Pattern
```java
Order testOrder = OrderBuilder.anOrder()
    .withId(1L)
    .withStatus(OrderStatus.PENDING)
    .withItems(List.of(testItem))
    .build();
```

### Test Fixtures
```java
@BeforeEach
void setUp() {
    testCustomer = CustomerFixtures.aValidCustomer();
    testOrder = OrderFixtures.aPendingOrder(testCustomer);
}
```

## Coverage Guidelines

| Type | Target |
|------|--------|
| Line Coverage | > 80% |
| Branch Coverage | > 70% |
| Critical Paths | 100% |

### What to Cover
- Business logic (always)
- Edge cases (null, empty, boundary)
- Error paths (exceptions, failures)
- State transitions

### What Not to Cover
- Getters/setters (unless logic)
- Framework code
- Generated code
- Configuration classes

## Testing Async Code

```java
@Test
void shouldProcessAsync() {
    CompletableFuture<Result> future = service.processAsync(input);

    Result result = future.get(5, TimeUnit.SECONDS);

    assertThat(result.isSuccess()).isTrue();
}
```

## Testing Temporal Workflows

```java
@Test
void shouldCompleteWorkflow() {
    WorkflowClient client = testEnvironment.getWorkflowClient();
    MyWorkflow workflow = client.newWorkflowStub(MyWorkflow.class);

    workflow.execute(input);

    assertThat(workflow.getState()).isEqualTo(WorkflowState.COMPLETED);
}
```

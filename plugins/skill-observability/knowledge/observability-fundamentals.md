# Observability Fundamentals

## Three Pillars

### 1. Logs
Discrete events that happened in the system.

### 2. Metrics
Numeric measurements over time.

### 3. Traces
Request flow across services.

## Logging Best Practices

### Structured Logging
```java
// CORRECT - Structured, searchable
logger.info("Order processed", Map.of(
    "orderId", orderId,
    "appId", appId,
    "amount", amount,
    "duration_ms", duration
));

// WRONG - Unstructured, hard to search
logger.info("Processed order " + orderId + " for app " + appId);
```

### Log Levels

| Level | Use For |
|-------|---------|
| ERROR | Failures requiring attention |
| WARN | Unexpected but handled conditions |
| INFO | Business events, state changes |
| DEBUG | Development troubleshooting |
| TRACE | Detailed execution flow |

### What to Log
- Request received (INFO)
- Request completed with duration (INFO)
- Business events (INFO)
- Errors with context (ERROR)
- External service calls (DEBUG)

### What NOT to Log
- Passwords, tokens, API keys
- Full credit card numbers
- Personal data (mask it)
- Successful health checks (too noisy)

## Error Handling

### Include Context
```java
try {
    processOrder(orderId);
} catch (OrderProcessingException e) {
    logger.error("Failed to process order", Map.of(
        "orderId", orderId,
        "appId", appId,
        "error", e.getMessage(),
        "errorType", e.getClass().getSimpleName()
    ));
    throw new ServiceException("Order processing failed", e);
}
```

### Error Response Pattern
```java
{
    "error": {
        "code": "ORDER_NOT_FOUND",
        "message": "Order with ID 123 not found",
        "request_id": "abc-123",
        "timestamp": "2024-01-01T00:00:00Z"
    }
}
```

## Metrics

### Key Metrics to Track

| Category | Metrics |
|----------|---------|
| Latency | p50, p95, p99 response times |
| Traffic | Requests per second |
| Errors | Error rate, error count by type |
| Saturation | CPU, memory, connections |

### Metric Naming
```
# Pattern: [namespace]_[subsystem]_[metric]_[unit]
violet_orders_processed_total
violet_orders_processing_duration_seconds
violet_api_requests_total
violet_api_errors_total
```

## Tracing

### Correlation IDs
```java
// Pass correlation ID through request chain
String correlationId = request.getHeader("X-Correlation-ID");
if (correlationId == null) {
    correlationId = UUID.randomUUID().toString();
}
MDC.put("correlationId", correlationId);
```

### Span Context
```java
Span span = tracer.spanBuilder("processOrder")
    .setAttribute("orderId", orderId)
    .setAttribute("appId", appId)
    .startSpan();

try (Scope scope = span.makeCurrent()) {
    // Processing logic
} finally {
    span.end();
}
```

## Alerting Guidelines

### Alert on Symptoms, Not Causes
```yaml
# GOOD - User-facing symptom
alert: HighErrorRate
expr: error_rate > 0.05  # 5% errors

# AVOID - Internal cause
alert: DatabaseConnectionPoolExhausted
expr: db_connections >= max_connections
```

### Alert Severity

| Severity | Response | Examples |
|----------|----------|----------|
| Critical | Immediate | Service down, data loss |
| High | Within hours | Elevated errors, degraded |
| Medium | Next business day | Warnings, approaching limits |
| Low | When convenient | Informational |

## Health Checks

### Liveness vs Readiness
```java
// Liveness: Is the process running?
@GetMapping("/health/live")
public ResponseEntity<?> liveness() {
    return ResponseEntity.ok().build();
}

// Readiness: Can it accept traffic?
@GetMapping("/health/ready")
public ResponseEntity<?> readiness() {
    if (databaseHealthy && cacheHealthy) {
        return ResponseEntity.ok().build();
    }
    return ResponseEntity.status(503).build();
}
```

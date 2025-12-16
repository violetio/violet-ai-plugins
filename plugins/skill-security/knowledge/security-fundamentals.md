# Security Fundamentals

## Core Principles

### Defense in Depth
Multiple layers of security controls:
- Network security (firewalls, VPCs)
- Application security (input validation, output encoding)
- Data security (encryption at rest and in transit)
- Identity security (authentication, authorization)

### Least Privilege
Grant minimum permissions necessary:
- Service accounts with scoped permissions
- Role-based access control (RBAC)
- Time-limited credentials
- Regular permission audits

### Secure by Default
Systems should be secure without additional configuration:
- Deny by default, allow by exception
- Secure defaults in configuration
- Fail securely (deny access on error)

## Input Validation

### Always Validate
```java
// CORRECT - Validate all input
public void processOrder(OrderRequest request) {
    Objects.requireNonNull(request, "Request cannot be null");
    validateOrderId(request.getOrderId());
    validateAmount(request.getAmount());
    // Process...
}

// WRONG - Trusting input
public void processOrder(OrderRequest request) {
    repository.findById(request.getOrderId()); // SQL injection risk
}
```

### Validation Patterns
- Whitelist over blacklist
- Validate type, length, format, range
- Sanitize before storage
- Encode before output

## Authentication

### Token Handling
```java
// Headers for authentication only
@RequestHeader("X-Violet-Token") String token      // Auth verification
@RequestHeader("X-Violet-App-Id") Integer appId    // Auth context

// Path parameters for resource identification
@PathVariable("app_id") Integer targetAppId         // Database queries
```

### Credential Storage
- Never store plaintext passwords
- Use encryption for API keys and tokens
- Rotate credentials regularly
- Audit credential access

## Authorization

### Check at Every Layer
```java
// Controller level
@PreAuthorize("hasRole('ADMIN')")
public ResponseEntity<?> adminEndpoint() { }

// Service level
if (!userService.canAccessResource(userId, resourceId)) {
    throw new AccessDeniedException();
}

// Data level (row-level security)
repository.findByIdAndAppId(id, authenticatedAppId);
```

## Data Protection

### Encryption
- **At rest**: AWS KMS, AES-256
- **In transit**: TLS 1.3
- **Sensitive fields**: Encrypted in database

### PII Handling
- Minimize collection
- Mask in logs (email → e***@example.com)
- Encrypt in storage
- Define retention policies

## Logging & Monitoring

### Security Logging
```java
// Log security events
logger.info("Authentication successful",
    Map.of("userId", userId, "ip", clientIp));

logger.warn("Failed login attempt",
    Map.of("username", username, "ip", clientIp, "attempts", attemptCount));

// Never log sensitive data
// WRONG: logger.info("Token: " + token);
```

### Monitoring
- Failed authentication attempts
- Unusual access patterns
- Privilege escalation attempts
- Data exfiltration indicators

## OWASP Top 10 Awareness

| Risk | Mitigation |
|------|------------|
| Injection | Parameterized queries, input validation |
| Broken Auth | Strong session management, MFA |
| Sensitive Data | Encryption, access controls |
| XXE | Disable external entities |
| Broken Access | Authorization at every layer |
| Security Misconfig | Secure defaults, hardening |
| XSS | Output encoding, CSP |
| Insecure Deserialization | Validate before deserializing |
| Vulnerable Components | Dependency scanning |
| Insufficient Logging | Comprehensive audit logs |

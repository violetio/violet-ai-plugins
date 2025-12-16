# Java/Spring Boot Conventions

## Stack Overview
- **Framework**: Spring Boot 3.2.x with Spring MVC
- **Java Version**: Java 21
- **Build**: Maven
- **Database**: MongoDB (Spring Data MongoDB) or PostgreSQL (Spring Data JPA)
- **Serialization**: Jackson with SNAKE_CASE property naming

## Project Structure
```
src/main/java/io/drizzl/platform/{service}/
├── config/         # Spring beans, configuration classes
├── temporal/       # Workflow and activity implementations
├── service/        # Core business logic
├── repository/     # Data access layer
├── web/            # REST controllers, webhook endpoints
├── model/          # Domain models and DTOs
└── {platform}/     # Platform-specific integrations (e.g., shopify/)
```

## Critical Patterns

### API Endpoint Parameter Pattern
**CRITICAL**: Always use path parameters for database queries, NOT header parameters!

```java
// CORRECT - Path parameter for database queries
@PostMapping("/apps/{app_id}/some-operation")
public ResponseEntity<?> someOperation(
    @RequestHeader("X-Violet-Token") String token,           // Auth only
    @RequestHeader("X-Violet-App-Secret") String appSecret,  // Auth only
    @RequestHeader("X-Violet-App-Id") Integer appIdHeader,   // Auth only
    @PathVariable("app_id") Integer appId,                   // USE THIS for DB
    @RequestBody SomeRequest request) {
    repository.findByAppId(appId);  // Use path parameter
}

// WRONG - Using header for database queries
@PostMapping("/some-operation")
public ResponseEntity<?> someOperation(
    @RequestHeader("X-Violet-App-Id") Integer appId) {
    repository.findByAppId(appId);  // Security risk!
}
```

**Standard Violet Endpoint Pattern:**
```
POST /v1/{service}/apps/{app_id}/endpoint-name
  Headers: X-Violet-Token, X-Violet-App-Secret, X-Violet-App-Id (authentication)
  Path: app_id (target for database operations)
  Query: Optional parameters (filters, dry_run, etc.)
  Body: Request payload
```

### Null Safety for Partial Updates
Always verify nested fields are not null before accessing:

```java
// WRONG - Assumes field is always present
if (updateDetails.getChanges() != null) {
    input.put("value", updateDetails.getChanges().getValue().getNewValue());
}

// CORRECT - Check nested field first
if (updateDetails.getChanges() != null
    && updateDetails.getChanges().getValue() != null) {
    input.put("value", updateDetails.getChanges().getValue().getNewValue());
}
```

### Database Timing Pass-Through Pattern
Fresh database queries immediately after save can retrieve stale data:

```java
// CORRECT - Pass-through pattern
Entity updated = repository.save(entity);
processWithUpdatedData(updated);  // Use passed object, not fresh query
```

## MongoDB Conventions
- Use `@Document` annotation instead of `@Entity`
- Use `@Field` instead of `@Column`
- Repository extends `MongoRepository`
- Use `@CreatedDate` / `@LastModifiedDate` for timestamps
- **Snake_case field naming** with `@Field` annotations

```java
@Document(collection = "my_collection")
public class MyDocument {
    @Id
    private String id;

    @Field("app_id")
    private Integer appId;

    @CreatedDate
    @Field("created_at")
    private Instant createdAt;
}
```

## Jackson Configuration
```properties
spring.jackson.property-naming-strategy=SNAKE_CASE
```

## Encryption
- **Local/Development**: AES encryption with test keys
- **Production**: AWS KMS (when `violet.env=production`)

```properties
security.aes.key=${AES_KEY:defaultKey}
security.aes.vector=${AES_VECTOR:defaultVector}
```

## Build & Run
```bash
mvn clean install spring-boot:run
```

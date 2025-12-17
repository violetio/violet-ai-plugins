# Shopify Best Practices & Common Issues

## Webhook Signature Validation

Always validate webhook signatures to ensure authenticity:

```java
// Always validate webhook signatures
String calculatedSignature = ShopifyWebhookUtils.calculateSignature(payload, webhookSecret);
if (!ShopifyWebhookUtils.isValidSignature(receivedSignature, calculatedSignature)) {
  throw new InvalidWebhookSignatureException();
}
```

## Rate Limiting Strategy

- **GraphQL**: 1000 points/sec (preferred for complex queries)
- **REST**: 40 calls/sec (legacy operations only)
- Implement exponential backoff with jitter
- Monitor `X-Rate-Limit-Remaining` headers in responses
- Burst limits: REST has 80 calls burst, GraphQL has 2000 points burst

## App Scope Management

```java
// Check required scopes before API calls
List<String> requiredScopes = Arrays.asList("read_products", "write_orders");
if (!ShopifyScopeUtils.hasRequiredScopes(merchant, requiredScopes)) {
  throw new InsufficientScopesException();
}
```

## Common Issues & Solutions

### GraphQL vs REST API Decision
- **Prefer GraphQL** for new implementations (better performance, type safety)
- **Use REST** only for operations not supported in GraphQL (legacy apps, specific operations)
- **Migrate legacy REST** implementations to GraphQL when possible
- GraphQL batching reduces rate limit impact

### Product Variant Handling
- Shopify has complex variant structures (options + values)
- Variant limits: Max 3 options, Max 100 variants per product
- Careful mapping required to Violet's variant system
- Test variant creation with edge cases (all 3 options, custom options)

### Inventory Location Management
- Shopify supports multiple inventory locations
- Handle location-specific stock quantities
- Consider fulfillment location preferences in orders
- Track inventory changes across locations

### Metafield Namespace Management
- Use consistent namespacing for Violet-specific data
- `violet.offer_id`, `violet.mapping_id`, etc.
- Be aware of metafield storage limits
- Use metafield versioning for breaking changes

### Currency Handling
- Shopify supports presentment currencies (store currency + display currency)
- Handle exchange rates for multi-currency orders
- Use `PriceUtils.centsToDollars()` for accurate conversions
- Test with edge cases: zero-decimal currencies, high-precision currencies

### Checkout & Payment
- Support for Shopify Payments and external gateways
- Handle payment intent flows
- Support for multi-currency checkout
- Test with different payment methods

## Testing Patterns

### Test Structure
- Tests located in `src/test/java/io/drizzl/platform/ecom/platforms/shopify/`
- Use `@SpringBootTest` with Shopify-specific mocks
- Comprehensive GraphQL response mocking

### Mock Examples
```java
@MockBean
private ShopifyServiceClientFactory shopifyServiceClientFactory;

@MockBean
private ShopifyGraphUtils shopifyGraphUtils;

// Mock GraphQL responses
when(shopifyGraphUtils.executeQuery(any(), any()))
    .thenReturn(mockShopifyGraphqlResponse);
```

### Test Data
- JSON fixtures in `src/test/resources/platform/shopify/`
- Realistic Shopify API response structures
- Both REST and GraphQL response formats
- Edge cases: null variants, missing fields, large datasets

## Debugging Tools

### GraphQL Query Testing
- Use Shopify GraphiQL explorer for query development
- Test queries with actual merchant data
- Validate query complexity and performance
- Check query depth (Shopify limits query depth for performance)

### Webhook Development
- Use ngrok for local webhook development
- Validate webhook signatures in development
- Test webhook retry behavior
- Monitor webhook delivery timeline

### API Client Testing
```java
// Use proxy endpoints for direct API testing
String proxyUrl = "/proxy/shopify/admin/api/2023-04/products.json";
// Test authentication and API responses
```

## Performance Optimization

### GraphQL Query Optimization
- Batch multiple operations into single queries
- Request only needed fields
- Use aliases to avoid multiple queries for similar data
- Monitor query complexity score

### Rate Limit Optimization
- Use GraphQL for complex operations (better rate limiting)
- Batch REST API calls when necessary
- Implement caching for frequently accessed data
- Monitor rate limit headers and adjust request patterns

### Connection Management
- Reuse API client instances
- Implement connection pooling
- Handle graceful degradation under high load

## Version Management

- Stay current with Shopify API versions
- Shopify deprecates API versions after 1 year
- Test compatibility with API version changes
- Document version-specific behavior

## Security Best Practices

- Never log webhook secrets or access tokens
- Use environment variables for credentials
- Validate all webhook signatures
- Implement API rate limiting detection and alerting
- Use TLS 1.2+ for all API calls
- Validate SSL certificates

## Integration Checklist

- [ ] GraphQL queries optimized for rate limits
- [ ] Webhook signature validation implemented
- [ ] Proper error handling for API failures
- [ ] Rate limiting with exponential backoff
- [ ] Metafield namespacing consistent
- [ ] Currency handling tested with edge cases
- [ ] Inventory location management properly scoped
- [ ] Static architecture patterns followed
- [ ] Comprehensive test coverage with mocks
- [ ] Production monitoring and alerting configured

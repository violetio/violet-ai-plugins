---
name: shopify
description: Shopify API patterns, GraphQL, webhooks
---

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

## Regional Pricing (Shopify Markets)

Shopify Markets allows merchants to set different prices per country/region using Price Lists and Catalogs.

### Querying Regional Prices (Admin API)

Use `contextualPricing` on ProductVariant to get region-specific prices:

```graphql
{
  product(id: "gid://shopify/Product/123") {
    variants(first: 10) {
      edges {
        node {
          price              # Base price (store default currency)
          compareAtPrice     # Base compare-at price

          # Regional pricing - pass country code
          contextualPricing(context: { country: GB }) {
            price { amount currencyCode }
            compareAtPrice { amount currencyCode }
          }
        }
      }
    }
  }
}
```

### Key Points
- **Base prices** (`price`, `compareAtPrice`) return the store's default currency
- **Contextual prices** return the regional currency and adjusted amounts
- Common country codes: `US`, `CA`, `GB`, `DE`, `FR`, `AU`
- Regional prices may differ due to currency conversion, regional adjustments, or region-specific price lists

### Storefront API Context

For Storefront API, use the `@inContext` directive:

```graphql
query @inContext(country: GB) {
  product(handle: "example") {
    variants(first: 10) {
      edges {
        node {
          price { amount currencyCode }
          compareAtPrice { amount currencyCode }
        }
      }
    }
  }
}
```

## Tax-Inclusive Pricing Detection

Different regions handle sales tax differently:

| Region | Tax Behavior | Example |
|--------|--------------|---------|
| US | **Exclusive** | Price $50 + tax at checkout |
| CA | **Exclusive** | Price $50 CAD + tax at checkout |
| UK | **Inclusive** | Price £50 includes VAT |
| EU (DE, FR, etc.) | **Inclusive** | Price €50 includes VAT |
| AU | **Inclusive** | Price $50 AUD includes GST |

### Detecting Tax-Inclusive Orders (Admin API)

Use `draftOrderCalculate` mutation to detect if taxes are included:

```graphql
mutation {
  draftOrderCalculate(input: {
    email: "customer@example.com"
    lineItems: [{
      variantId: "gid://shopify/ProductVariant/123"
      quantity: 1
    }]
    shippingAddress: {
      address1: "123 Example St"
      city: "London"
      zip: "SW1A 1AA"
      countryCode: GB
    }
    presentmentCurrencyCode: GBP
  }) {
    calculatedDraftOrder {
      taxesIncluded          # KEY FIELD: true = tax-inclusive pricing
      presentmentCurrencyCode
      totalPriceSet { presentmentMoney { amount currencyCode } }
      totalTaxSet { presentmentMoney { amount currencyCode } }
      taxLines { title rate priceSet { presentmentMoney { amount currencyCode } } }
    }
    userErrors { field message }
  }
}
```

### Required Input for Accurate Tax Calculation

| Field | Required | Purpose |
|-------|----------|---------|
| `lineItems[].variantId` | Yes | Product being purchased |
| `shippingAddress.countryCode` | Yes | Determines tax jurisdiction |
| `shippingAddress.zip` | Recommended | Refines tax rate |
| `shippingAddress.provinceCode` | For US/CA | State/province tax rates |
| `presentmentCurrencyCode` | Recommended | Display currency |

### Third-Party Tax Providers (Avalara)

When merchants use Avalara or other tax apps:
- `draftOrderCalculate` may use Shopify's fallback rates instead of Avalara
- Storefront checkout may behave differently than Admin API
- "Dynamic tax display" settings can affect API responses
- Always verify API responses match expected storefront behavior

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

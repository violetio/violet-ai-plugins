# Shopify Platform - CLAUDE.md

Platform-specific guidance for Shopify integration development.

## Platform Overview

Shopify is a leading ecommerce platform with comprehensive GraphQL and REST API support. This integration supports both public and custom app installations with extensive webhook capabilities.

### Key Features
- **GraphQL API**: Primary API for modern integrations with complex queries
- **REST API**: Legacy support for specific operations
- **Custom App Scopes**: Granular permission system
- **Storefront API**: Customer-facing operations
- **Webhook System**: Real-time event notifications
- **Multi-channel Support**: Online store, POS, social commerce

## Architecture Overview

### Core Services
- `ShopifyOrderSyncService`: Order synchronization and management
- `ShopifyProductSyncService`: Product catalog operations
- `ShopifyCustomerSyncService`: Customer data management
- `ShopifyShopSyncService`: Shop configuration and settings
- `ShopifyServiceClientFactory`: Authentication and API client management
- `ShopifyWebhookService`: Webhook event processing

### Key Components

**Composers (Violet → Shopify)**
- `ShopifyGraphOrderComposer`: Modern GraphQL-based order creation (recommended)
- `ShopifyOrderComposer`: Legacy REST API order creation
- `ShopifyDraftOrderComposer`: Draft order management
- `ShopifyStorefrontCartComposer`: Customer cart operations
- `ShopifyProductComposer`: Modern static product composition for product creation/updates

**Decomposers (Shopify → Violet)**
- `ShopifyGraphProductDecomposer`: Modern static product decomposition
- `ShopifyProductDecomposer`: Legacy product decomposition
- `ShopifyOrderDecomposer`: Order data transformation
- `ShopifyCustomerDecomposer`: Customer data transformation
- `ShopifyCollectionDecomposer`: Product collection handling

**Utilities**
- `ShopifyGraphUtils`: GraphQL query building and execution
- `ShopifyGraphOrderComposerUtils`: Order composition helper methods
- `ShopifyAuthUtils`: Authentication and token management
- `ShopifyErrorUtils`: Error handling and retry logic
- `ShopifyWebhookUtils`: Webhook signature validation

## Development Patterns

### GraphQL Order Composer (Modern Approach)

The `ShopifyGraphOrderComposer` follows modern static architecture:

```java
public class ShopifyGraphOrderComposer {
  public static DraftOrderInput composeOrder(
      TransactableBag transactableBag,
      MerchantConfiguration merchantConfig,
      AppConfiguration appConfig,
      String violetEnv) {

    // 1. Environment-specific configuration
    if (StringUtils.isNotEmpty(violetEnv) && !"prod".equals(violetEnv)) {
      // Apply test flags for non-production
    }

    // 2. Apply order components using utility methods
    ShopifyGraphOrderComposerUtils.applyLineItemsToOrder(transactableBag, draftOrderInput);
    ShopifyGraphOrderComposerUtils.applyCustomerInfo(transactableBag, draftOrderInput);
    ShopifyGraphOrderComposerUtils.applyAddresses(transactableBag, draftOrderInput);

    return draftOrderInput;
  }
}
```

### Product Decomposer (Modern Static)

```java
public class ShopifyGraphProductDecomposer {
  public static Offer decompose(ShopifyProduct shopifyProduct, Merchant merchant, ShopifyOfferSyncConfig config) {
    Offer offer = new Offer();

    // Use helper methods for component population
    populateOfferBasics(shopifyProduct, offer, merchant);
    populateOfferMedia(shopifyProduct, offer);
    populateOfferVariants(shopifyProduct, offer);
    populateOfferSkus(shopifyProduct, offer, merchant, config);

    return offer;
  }
}
```

### Advanced Patterns

**GraphQL Query Building**
```java
// Use ShopifyGqlQueries for consistent query construction
String productQuery = ShopifyGqlQueries.getProductByIdQuery(productId, includeVariants);
ShopifyGraphqlResponse response = shopifyGraphUtils.executeQuery(query, variables);
```

**Exchange Rate Handling**
```java
// Shopify supports presentment currencies - handle exchange rates
if (Objects.nonNull(transactableBag.getCurrencyExchangeRate())) {
  BigDecimal invertedRate = OrderUtils.invertExchangeRate(transactableBag.getCurrencyExchangeRate());
  ShopifyGraphOrderComposerUtils.applyExchangeRate(draftOrderInput, invertedRate);
}
```

**Inventory Behavior Settings**
```java
// Configure inventory behavior for GraphQL orders
draftOrderInput.setInventoryBehaviour(InventoryBehaviour.DECREMENT_IGNORING_POLICY);
```

**Error Handling**
```java
// Use Shopify-specific error handling
try {
  // Shopify API call
} catch (ShopifyApiException e) {
  ShopifyErrorUtils.handleApiError(e, merchantId);
  throw new PlatformSyncException("Shopify sync failed", e);
}
```

### Product Composer (Modern Static)

The `ShopifyProductComposer` transforms Violet Offers into Shopify Products for creation/update operations:

```java
public class ShopifyProductComposer {
  public static ShopifyProduct composeProduct(
      Offer offer,
      Merchant merchant,
      ShopifyProductSyncService.ShopifyOfferSyncConfig config) {

    ShopifyProduct shopifyProduct = new ShopifyProduct();

    // Use helper methods for component composition
    composeProductBasics(offer, shopifyProduct, merchant);
    composeProductImages(offer, shopifyProduct);
    composeProductVariants(offer, shopifyProduct);
    composeProductMetafields(offer, shopifyProduct);
    applyConfigurationOverrides(shopifyProduct, config);

    return shopifyProduct;
  }
}
```

**Key Composition Patterns**:
- **Product Status Mapping**: `Offer.Status.ARCHIVED` → `Product.Status.archived`, availability + visibility → `active`
- **Variant Mapping**: Violet SKUs → Shopify Variants with proper option1/option2/option3 assignment
- **Media Transformation**: Violet Albums → Shopify Images with position and external ID preservation
- **Metafield Conversion**: Violet MetaObjects → Shopify Metafields with type mapping
- **Pricing**: Use `PriceUtils.centsToDollars()` for price conversion from Violet to Shopify
- **Inventory Management**: Map `inventoryTracked` boolean to Shopify's `inventory_policy` ("deny"/"continue")

**Product Type Handling**:
```java
private static Boolean determineRequiresShipping(Product.Type productType) {
  return switch (productType) {
    case DIGITAL -> false;
    case PHYSICAL, BUNDLED -> true;
    default -> true;
  };
}
```

**Variant Option Mapping**:
```java
// Shopify supports max 3 options - map SKU variant values to option1/option2/option3
switch (optionIndex) {
  case 0 -> shopifyVariant.setOption1(optionValue);
  case 1 -> shopifyVariant.setOption2(optionValue);
  case 2 -> shopifyVariant.setOption3(optionValue);
}
```

## Testing Patterns

### Test Structure
- Tests located in `src/test/java/io/drizzl/platform/ecom/platforms/shopify/`
- Use `@SpringBootTest` with Shopify-specific mocks
- Comprehensive GraphQL response mocking with WireMock

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

## Common Issues & Solutions

### GraphQL vs REST API
- **Prefer GraphQL** for new implementations (better performance, type safety)
- **Use REST** only for operations not supported in GraphQL
- **Migrate legacy REST** implementations to GraphQL when possible

### Webhook Signature Validation
```java
// Always validate webhook signatures
String calculatedSignature = ShopifyWebhookUtils.calculateSignature(payload, webhookSecret);
if (!ShopifyWebhookUtils.isValidSignature(receivedSignature, calculatedSignature)) {
  throw new InvalidWebhookSignatureException();
}
```

### Rate Limiting
- Shopify has strict rate limits (40 calls/sec for REST, 1000 points/sec for GraphQL)
- Use exponential backoff with jitter
- Monitor rate limit headers in responses

### App Scopes & Permissions
```java
// Check required scopes before API calls
List<String> requiredScopes = Arrays.asList("read_products", "write_orders");
if (!ShopifyScopeUtils.hasRequiredScopes(merchant, requiredScopes)) {
  throw new InsufficientScopesException();
}
```

## Platform-Specific Considerations

### Multi-Location Inventory
- Shopify supports multiple inventory locations
- Handle location-specific stock quantities
- Consider fulfillment location preferences

### Product Variants
- Shopify has complex variant structures (options + values)
- Map carefully to Violet's variant system
- Handle variant limits (3 options max, 100 variants max per product)

### Metafields
- Extensive metafield system for custom data
- Use for storing Violet-specific references
- Namespace metafields properly (`violet.offer_id`, etc.)

### Checkout & Payment
- Support for Shopify Payments and external gateways
- Handle payment intent flows
- Support for multi-currency checkout

## Migration Guidelines

### Legacy to Modern Migration
1. **Identify Legacy Components**: Look for instance-based composers/decomposers
2. **Create Static Equivalents**: Extract logic into static utility methods
3. **Update Tests**: Migrate to static method testing patterns
4. **Performance Testing**: Verify GraphQL performance improvements

### REST to GraphQL Migration
1. **Audit REST Usage**: Identify REST API calls that can be replaced
2. **Query Optimization**: Combine multiple REST calls into single GraphQL queries
3. **Error Handling**: Update error handling for GraphQL response format
4. **Rate Limit Benefits**: Leverage improved rate limiting with GraphQL

## Debugging Tools

### GraphQL Query Testing
- Use Shopify GraphiQL explorer for query development
- Test queries with actual merchant data
- Validate query complexity and performance

### Webhook Testing
- Use ngrok for local webhook development
- Validate webhook signatures in development
- Test webhook retry behavior

### API Client Testing
```java
// Use proxy endpoints for direct API testing
String proxyUrl = "/proxy/shopify/admin/api/2023-04/products.json";
// Test authentication and API responses
```

This documentation should be updated as new Shopify features are implemented or API versions change.

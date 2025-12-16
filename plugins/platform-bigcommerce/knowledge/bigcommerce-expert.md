# BigCommerce Platform - CLAUDE.md

Platform-specific guidance for BigCommerce integration development.

## Platform Overview

BigCommerce is an enterprise-focused ecommerce platform with comprehensive REST API support and advanced webhook capabilities. The platform is designed for larger merchants with complex requirements.

### Key Features
- **Enterprise Focus**: Built for high-volume, complex commerce operations
- **REST API**: Comprehensive REST API with extensive documentation
- **Webhook System**: Real-time event notifications with retry logic
- **Product Variants**: Complex product option and variant system
- **Multi-Channel**: Headless commerce and API-first architecture
- **Advanced Catalog**: Complex product relationships and categories
- **Extensive Metadata**: Rich product and order metadata system
- **Payment Gateways**: Support for multiple payment providers

## Architecture Overview

### Core Services
- `BigCommerceOrderSyncService`: Order synchronization and management
- `BigCommerceProductSyncService`: Product catalog operations
- `BigCommerceCustomerSyncService`: Customer data management
- `BigCommerceShippingSyncService`: Shipping method and zone management
- `BigCommerceShopSyncService`: Shop configuration and settings
- `BigCommerceServiceClientFactory`: Authentication and API client management
- `BigcommerceWebhookSyncService`: Webhook event processing

### Key Components

**Composers (Violet → BigCommerce)**
- `BigcommerceOrderComposer`: Legacy instance-based order composition

**Decomposers (BigCommerce → Violet)**
- `BigCommerceProductDecomposer`: Legacy instance-based product decomposition
- `BigCommerceOrderDecomposer`: Order data transformation
- `BigCommerceCustomerDecomposer`: Customer data transformation

**Utilities**
- `BigCommerceCalculateCartUtils`: Cart and pricing calculations
- `BigCommerceCartUtils`: Cart management utilities
- `BigCommerceCommonUtils`: Shared utility functions
- `BigCommerceDiscountUtils`: Discount and coupon utilities
- `BigCommerceOrderDecomposerUtils`: Order decomposition helpers
- `BigCommerceOrderUtils`: Order processing utilities
- `BigCommerceProductDecomposerUtils`: Product decomposition utilities
- `BigCommerceProductSyncUtils`: Product synchronization utilities
- `BigCommerceShippingRelayUtils`: Shipping calculation helpers
- `BigCommerceWebhookUtils`: Webhook processing utilities

**Async Processing**
- `AsyncBcOrderEvents`: Asynchronous order event processing

## Development Patterns

### Order Composer (Legacy Instance Architecture)

**Note**: BigCommerce uses legacy instance architecture and should be migrated to static methods.

```java
public class BigcommerceOrderComposer {
  // Legacy pattern - should be migrated to static
  private final BigCommerceOrderUtils orderUtils;

  public BigCommerceOrder composeOrder(TransactableBag transactableBag, MerchantConfiguration merchantConfig) {
    BigCommerceOrder bcOrder = new BigCommerceOrder();

    // Comprehensive order structure with extensive metadata
    applyExtensiveMetadata(transactableBag, bcOrder);

    // Product option mapping for variants
    applyProductOptions(transactableBag, bcOrder);

    // Support for coupons and discount codes
    applyCouponsAndDiscounts(transactableBag, bcOrder);

    // Multiple payment methods
    applyPaymentMethods(transactableBag, bcOrder);

    // Numeric status IDs
    applyNumericStatusIds(transactableBag, bcOrder);

    return bcOrder;
  }
}
```

### Product Decomposer (Legacy Pattern)

**Note**: BigCommerce uses legacy instance architecture that should be migrated.

```java
public class BigCommerceProductDecomposer {
  // Legacy instance variables - should be removed in migration
  private final BigCommerceProductDecomposerUtils utils;
  private final BigCommerceCommonUtils commonUtils;

  public Offer decompose(BigCommerceProduct product, Merchant merchant) {
    // Legacy instance method - should be static
    Offer offer = new Offer();

    // Complex product option handling
    handleProductOptions(product, offer);

    // Extensive metadata processing
    processExtensiveMetadata(product, offer);

    // Multiple payment methods support
    configurePaymentMethods(product, offer);

    return offer;
  }
}
```

### Product Option Mapping

BigCommerce has a complex product option system:

```java
private void handleProductOptions(BigCommerceProduct product, Offer offer) {
  if (CollectionUtils.isNotEmpty(product.getVariants())) {
    // Create Violet variants from BigCommerce options
    Map<String, Variant> violetVariants = new HashMap<>();

    for (BigCommerceVariant bcVariant : product.getVariants()) {
      for (BigCommerceOptionValue optionValue : bcVariant.getOptionValues()) {
        String optionName = optionValue.getOptionDisplayName();

        Variant violetVariant = violetVariants.computeIfAbsent(optionName, k -> {
          Variant v = new Variant();
          v.setName(k);
          v.setValues(new HashSet<>());
          return v;
        });

        VariantValue vv = new VariantValue();
        vv.setValue(optionValue.getValue());
        violetVariant.getValues().add(vv);
      }
    }

    offer.setVariants(new HashSet<>(violetVariants.values()));
  }
}
```

### Coupon and Discount Processing

```java
private void applyCouponsAndDiscounts(TransactableBag bag, BigCommerceOrder order) {
  // Apply coupon codes
  if (CollectionUtils.isNotEmpty(bag.getCoupons())) {
    for (Coupon coupon : bag.getCoupons()) {
      order.addCouponCode(coupon.getCode());
    }
  }

  // Apply discount amounts
  if (bag.getDiscountTotal() > 0) {
    BigCommerceDiscount discount = new BigCommerceDiscount();
    discount.setAmount(PriceUtils.centsToDollars(bag.getDiscountTotal()));
    discount.setType("fixed");
    order.getDiscounts().add(discount);
  }
}
```

### Extensive Metadata System

```java
private void applyExtensiveMetadata(TransactableBag bag, BigCommerceOrder order) {
  // Order-level metadata
  order.getCustomFields().put("violet_order_id", bag.getOrderId());
  order.getCustomFields().put("violet_bag_id", String.valueOf(bag.getId()));

  // Line item metadata
  for (OrderSku orderSku : bag.getSkus()) {
    BigCommerceLineItem lineItem = new BigCommerceLineItem();
    lineItem.getCustomFields().put("violet_sku_id", String.valueOf(orderSku.getId()));
    lineItem.getCustomFields().put("violet_external_id", orderSku.getExternalId());
    order.getLineItems().add(lineItem);
  }

  // Customer metadata
  if (Objects.nonNull(bag.getCustomer())) {
    order.getCustomFields().put("customer_notes", bag.getCustomer().getNotes());
  }
}
```

### Numeric Status IDs

BigCommerce uses numeric status IDs instead of strings:

```java
private void applyNumericStatusIds(TransactableBag bag, BigCommerceOrder order) {
  switch (bag.getStatus()) {
    case IN_PROGRESS:
      order.setStatusId(11); // Awaiting Fulfillment
      break;
    case ACCEPTED:
      order.setStatusId(2); // Awaiting Shipment
      break;
    case COMPLETED:
      order.setStatusId(10); // Completed
      break;
    case CANCELED:
      order.setStatusId(5); // Cancelled
      break;
    case REJECTED:
      order.setStatusId(6); // Declined
      break;
    default:
      order.setStatusId(1); // Pending
  }
}
```

## Testing Patterns

### Test Structure
- Tests located in `src/test/java/io/drizzl/platform/ecom/platforms/bigcommerce/`
- Use `@SpringBootTest` with BigCommerce-specific mocks
- JSON fixtures in `src/test/resources/platform/bigcommerce/`

### Mock Examples
```java
@MockBean
private BigCommerceServiceClientFactory bcServiceClientFactory;

@MockBean
private BigCommerceCalculateCartUtils cartCalculationUtils;

@MockBean
private AsyncBcOrderEvents asyncBcOrderEvents;

// Mock BigCommerce API responses
when(bcServiceClientFactory.getApiClient(merchantId))
    .thenReturn(mockBigCommerceClient);
```

### Test Data
- Complex product structures with variants and options
- Order responses with extensive metadata
- Webhook payloads with proper signatures

## Common Issues & Solutions

### Product Option Complexity
- BigCommerce has complex option/variant relationships
- Multiple option types (dropdown, radio, checkbox, etc.)
- Option value dependencies and pricing modifiers

### Webhook Signature Validation
```java
// Validate BigCommerce webhook signatures
String expectedSignature = BigCommerceWebhookUtils.calculateSignature(payload, webhookSecret);
if (!BigCommerceWebhookUtils.isValidSignature(receivedSignature, expectedSignature)) {
  throw new InvalidWebhookSignatureException();
}
```

### Rate Limiting
- BigCommerce has strict rate limits (450 requests per 30 seconds)
- Implement exponential backoff with jitter
- Monitor `X-Rate-Limit-Remaining` headers

### Authentication Issues
- **Store Hash**: Required for API authentication
- **Access Token**: OAuth token management
- **API Path**: Correct API version and path construction

## Platform-Specific Considerations

### Enterprise Features
- Advanced catalog management with categories and brands
- Customer groups and pricing tiers
- Multi-channel inventory management
- Advanced shipping and tax rules

### Product Variants
- Complex option system with dependencies
- Variant-specific pricing and inventory
- Image associations with variants
- SKU management for variants

### Order Management
- Comprehensive order lifecycle management
- Advanced fulfillment options
- Return and exchange processing
- Order notes and communications

### Payment Processing
- Multiple payment gateway support
- Stored payment methods
- Recurring billing capabilities
- Fraud detection integration

### Multi-Channel Support
- Headless commerce capabilities
- API-first architecture
- Channel-specific configurations
- Cross-channel inventory sync

## Migration Guidelines

### Legacy to Modern Migration
The BigCommerce integration needs significant modernization:

1. **Identify Legacy Components**:
   - `BigcommerceOrderComposer` (instance-based)
   - `BigCommerceProductDecomposer` (instance-based)

2. **Create Static Equivalents**:
   ```java
   // Target modern pattern
   public class BigcommerceOrderComposer {
     public static BigCommerceOrder composeOrder(TransactableBag bag, MerchantConfiguration config) {
       // Static method implementation
     }
   }
   ```

3. **Extract Utility Methods**:
   - Move complex logic to utility classes
   - Create focused helper methods for option mapping
   - Remove instance variable dependencies

4. **Update Tests**: Migrate to static method testing patterns

### API Version Migration
- Stay current with BigCommerce API versions
- Test compatibility with API changes
- Handle deprecation warnings and sunset dates

## Debugging Tools

### API Testing
```java
// Test BigCommerce API endpoints directly
String apiUrl = "https://api.bigcommerce.com/stores/{store_hash}/v3/products";
// Include proper authentication headers
```

### Webhook Development
- Use ngrok for local webhook testing
- Verify webhook signatures in development
- Test webhook retry behavior and failure handling

### Store Configuration
- Verify API credentials and permissions
- Check store-specific settings that affect API behavior
- Test with different store configurations

This platform requires careful handling of enterprise-level complexity and extensive metadata management.

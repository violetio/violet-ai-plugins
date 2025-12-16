# WooCommerce Platform - CLAUDE.md

Platform-specific guidance for WooCommerce integration development.

## Platform Overview

WooCommerce is a WordPress-based ecommerce platform that powers millions of online stores. The integration requires the WooCommerce plugin to be installed and properly configured on the WordPress site.

### Key Features
- **WordPress Integration**: Built on WordPress with extensive plugin ecosystem
- **REST API**: Comprehensive REST API for all ecommerce operations
- **Metadata System**: Extensive metadata support for custom properties
- **Product Variations**: Complex product variation system
- **Coupon System**: Advanced discount and coupon management
- **Shipping Zones**: Flexible shipping configuration
- **Tax Management**: Comprehensive tax calculation system
- **Multi-site Support**: WordPress multisite compatibility

## Architecture Overview

### Core Services
- `WooCommerceOrderSyncService`: Order synchronization and management
- `WooCommerceProductSyncService`: Product catalog operations
- `WooCommerceCustomerSyncService`: Customer data management
- `WooCommerceShippingSyncService`: Shipping method and zone management
- `WooCommerceShopSyncService`: Shop configuration and settings
- `WooCommerceTaxSyncService`: Tax rate and configuration management
- `WooCommerceServiceClientFactory`: Authentication and API client management
- `WooCommerceWebhookService`: Webhook event processing

### Key Components

**Composers (Violet → WooCommerce)**
- `WooCommerceOrderComposer`: Order creation with extensive metadata support

**Decomposers (WooCommerce → Violet)**
- `WooCommerceProductDecomposer`: Legacy instance-based product decomposition
- `WooProductV3Decomposer`: Version 3 API product decomposition
- `WooCommerceOrderDecomposer`: Order data transformation
- `WooCommerceCustomerDecomposer`: Customer data transformation

**Utilities**
- `WooCommerceCartCalculationUtils`: Cart and pricing calculations
- `WooCommerceCartUtils`: Cart management utilities
- `WooCommerceOrderUtils`: Order processing helpers
- `WooCommerceProductDecomposerUtils`: Product decomposition utilities
- `WooCommerceRelayShippingUtils`: Shipping calculation helpers
- `WooCommerceTaxUtils`: Tax calculation utilities
- `WooCommerceShopUtils`: Shop configuration utilities
- `WooProductSyncUtils`: Product synchronization utilities

## Development Patterns

### Order Composer (Legacy Instance Architecture)

**Note**: WooCommerce still uses legacy instance architecture and should be migrated to static methods.

```java
public class WooCommerceOrderComposer {
  // Legacy pattern - should be migrated to static
  public WooCommerceOrder composeOrder(TransactableBag transactableBag, MerchantConfiguration merchantConfig) {
    WooCommerceOrder wooOrder = new WooCommerceOrder();

    // Extensive metadata usage for custom properties and tracking
    applyMetadata(transactableBag, wooOrder);

    // Line-item level discount distribution
    applyLineItemDiscounts(transactableBag, wooOrder);

    // Coupon line creation for applied discounts
    applyCouponLines(transactableBag, wooOrder);

    // Digital-only order detection for shipping exemption
    if (!OrderUtils.allDigitalItems(transactableBag.getSkus())) {
      applyShippingAddress(transactableBag, wooOrder);
    }

    return wooOrder;
  }
}
```

### Metadata Patterns

WooCommerce extensively uses metadata for custom properties:

```java
private void applyMetadata(TransactableBag bag, WooCommerceOrder order) {
  // Violet-specific metadata
  order.addMetaData("violet_order_id", bag.getOrderId());
  order.addMetaData("violet_bag_id", bag.getId());

  // Tracking information
  if (StringUtils.isNotEmpty(bag.getTrackingNumber())) {
    order.addMetaData("tracking_number", bag.getTrackingNumber());
  }

  // Custom properties
  if (Objects.nonNull(bag.getCustomProperties())) {
    bag.getCustomProperties().forEach((key, value) ->
      order.addMetaData("custom_" + key, value)
    );
  }
}
```

### Coupon Line Creation

```java
private void applyCouponLines(TransactableBag bag, WooCommerceOrder order) {
  if (bag.getDiscountTotal() > 0) {
    WooCommerceCouponLine couponLine = new WooCommerceCouponLine();
    couponLine.setCode("VIOLET_DISCOUNT");
    couponLine.setDiscount(PriceUtils.centsToDollars(bag.getDiscountTotal()));
    couponLine.setDiscountTax("0.00");

    order.getCouponLines().add(couponLine);
  }
}
```

### Line-Item Discount Distribution

```java
private void applyLineItemDiscounts(TransactableBag bag, WooCommerceOrder order) {
  final Map<Long, Double> lineItemDiscounts =
      OrderUtils.getPerLineItemDiscountAmounts(bag);

  for (OrderSku orderSku : bag.getSkus()) {
    WooCommerceLineItem lineItem = new WooCommerceLineItem();
    lineItem.setProductId(orderSku.getExternalId());
    lineItem.setQuantity(orderSku.getQuantity());

    // Calculate line item total with discount
    int skuDiscount = 0;
    if (lineItemDiscounts.containsKey(orderSku.getId())) {
      skuDiscount = PriceUtils.dollarsToCents(lineItemDiscounts.get(orderSku.getId()));
    }
    int lineItemTotal = orderSku.getLinePrice() - skuDiscount;
    lineItem.setTotal(PriceUtils.centsToDollars(lineItemTotal));

    order.getLineItems().add(lineItem);
  }
}
```

### Product Decomposer (Legacy Pattern)

**Note**: WooCommerce uses legacy instance architecture that should be migrated.

```java
public class WooCommerceProductDecomposer {
  // Legacy instance variables - should be removed in migration
  private final WooCommerceProductDecomposerUtils utils;

  public Offer decompose(WooCommerceProduct product, Merchant merchant) {
    // Legacy instance method - should be static
    Offer offer = new Offer();

    // Handle product variations
    if (product.getType().equals("variable")) {
      populateVariableProduct(product, offer, merchant);
    } else {
      populateSimpleProduct(product, offer, merchant);
    }

    return offer;
  }
}
```

## Testing Patterns

### Test Structure
- Tests located in `src/test/java/io/drizzl/platform/ecom/platforms/woocommerce/`
- Use `@SpringBootTest` with WooCommerce-specific mocks
- JSON fixtures in `src/test/resources/platform/woocommerce/`

### Mock Examples
```java
@MockBean
private WooCommerceServiceClientFactory wooServiceClientFactory;

@MockBean
private WooCommerceCartCalculationUtils cartCalculationUtils;

// Mock WooCommerce API responses
when(wooServiceClientFactory.getApiClient(merchantId))
    .thenReturn(mockWooCommerceClient);
```

### Test Data
- Realistic WooCommerce product structures with variations
- Order responses with line items, coupons, and metadata
- Customer data with billing and shipping addresses

## Common Issues & Solutions

### WordPress Plugin Dependency
- **Required Plugin**: WooCommerce plugin must be installed and activated
- **Version Compatibility**: Ensure API version compatibility
- **Permission Issues**: Verify API keys have proper permissions

### Product Variation Complexity
```java
// Handle WooCommerce variation structure
if ("variable".equals(product.getType())) {
  // Parent product with variations
  for (WooCommerceVariation variation : product.getVariations()) {
    Sku sku = createSkuFromVariation(variation, merchant);
    offer.getSkus().add(sku);
  }
} else {
  // Simple product
  Sku sku = createSkuFromProduct(product, merchant);
  offer.getSkus().add(sku);
}
```

### Metadata Management
```java
// Extract custom data from metadata
String violetOrderId = extractMetaValue(order.getMetaData(), "violet_order_id");
String customProperty = extractMetaValue(order.getMetaData(), "custom_property");

private String extractMetaValue(List<WooCommerceMetaData> metaData, String key) {
  return metaData.stream()
      .filter(meta -> key.equals(meta.getKey()))
      .map(WooCommerceMetaData::getValue)
      .findFirst()
      .orElse(null);
}
```

### Authentication Issues
- **Consumer Key/Secret**: WooCommerce uses OAuth 1.0a authentication
- **HTTPS Requirement**: SSL required for secure key transmission
- **Permission Scopes**: Verify read/write permissions for required resources

## Platform-Specific Considerations

### WordPress Integration
- WooCommerce is a WordPress plugin, not a standalone platform
- Must respect WordPress conventions and limitations
- Plugin conflicts can affect API behavior

### Tax System
- Complex tax calculation system with multiple rates
- Tax-inclusive vs tax-exclusive pricing
- Geographic-based tax rules

### Shipping Zones
- Flexible shipping zone configuration
- Multiple shipping methods per zone
- Conditional shipping rules

### Product Types
- **Simple**: Standard products without variations
- **Variable**: Products with variations (size, color, etc.)
- **Grouped**: Related products sold together
- **External**: Products sold on external sites
- **Virtual**: Digital products
- **Downloadable**: Digital products with file downloads

### Currency Support
- Multi-currency support through plugins
- Base currency configuration
- Currency conversion handling

## Migration Guidelines

### Legacy to Modern Migration
The WooCommerce integration needs significant modernization:

1. **Identify Legacy Components**:
   - `WooCommerceOrderComposer` (instance-based)
   - `WooCommerceProductDecomposer` (instance-based)

2. **Create Static Equivalents**:
   ```java
   // Target modern pattern
   public class WooCommerceOrderComposer {
     public static WooCommerceOrder composeOrder(TransactableBag bag, MerchantConfiguration config) {
       // Static method implementation
     }
   }
   ```

3. **Extract Utility Methods**:
   - Move complex logic to utility classes
   - Create focused helper methods
   - Remove instance variable dependencies

4. **Update Tests**: Migrate to static method testing patterns

## WordPress-Specific Debugging

### API Endpoint Testing
```java
// Test WooCommerce API endpoints directly
String apiUrl = "https://yourstore.com/wp-json/wc/v3/products";
// Include authentication headers and test responses
```

### Plugin Conflicts
- Test with minimal plugin setup
- Identify conflicting plugins that affect API behavior
- Verify WooCommerce plugin version compatibility

### WordPress Multisite
- Handle multisite WordPress installations
- Ensure proper site-specific API endpoints
- Manage per-site authentication

This platform requires careful handling of WordPress-specific considerations and extensive metadata management.

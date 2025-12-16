# Rithum Platform - CLAUDE.md

Platform-specific guidance for Rithum integration development.

## Platform Overview

Rithum (formerly CommerceHub) is a B2B marketplace platform that connects retailers with suppliers. The platform specializes in drop-shipping and marketplace fulfillment with complex product catalog and order management systems.

### Key Features
- **Drop-shipping Focus**: Supplier-to-consumer fulfillment model
- **Complex Product Types**: Variation parents, children, and single products
- **Multi-tier Status System**: Checkout, payment, and shipping statuses
- **Comprehensive Order Management**: Detailed order lifecycle tracking
- **Supplier Network**: Integration with multiple suppliers and retailers
- **Inventory Management**: Real-time stock synchronization

## Architecture Overview

### Core Services
- `RithumOrderSyncService`: Order synchronization with fallback shipping methods
- `RithumProductSyncService`: Product catalog and variant management
- `RithumShopSyncService`: Shop configuration and supplier setup
- `RithumServiceClientFactory`: Authentication and API client management
- `RithumWebhookSyncService`: Event notification processing

### Key Components

**Composers (Violet → Rithum)**
- `RithumOrderComposer`: Modern static order composition with comprehensive mapping

**Decomposers (Rithum → Violet)**
- `RithumProductDecomposer`: Advanced product decomposition with parent-child relationships
- `RithumOrderDecomposer`: Order transformation with three-tier status mapping

**Utilities**
- `RithumCommonUtils`: Shared utility functions
- `RithumOrderComposerUtils`: Order composition helper methods
- `RithumOrderUtils`: Order processing utilities
- `RithumProductDecomposerUtils`: Product decomposition helpers
- `RithumSchedulingUtils`: Scheduling and timing utilities

## Development Patterns

### Order Composer (Modern Static Architecture)

The `RithumOrderComposer` exemplifies modern static architecture with comprehensive property mapping:

```java
public class RithumOrderComposer {
  public static RithumOrder composeOrder(TransactableBag transactableBag, MerchantConfiguration merchantConfig) {
    RithumOrder rithumOrder = new RithumOrder();

    // Process all order statuses, not just IN_PROGRESS
    if (Objects.isNull(transactableBag) || Objects.isNull(transactableBag.getStatus())) {
      return rithumOrder;
    }

    // Direct property assignment leveraging TransactableBag guarantees
    rithumOrder.setTotalPrice(PriceUtils.centsToDollars(transactableBag.getTotal()));
    rithumOrder.setTotalTax(PriceUtils.centsToDollars(transactableBag.getTaxTotal()));
    rithumOrder.setShippingTotal(PriceUtils.centsToDollars(transactableBag.getShippingTotal()));

    // Handle discounts as negative adjustments
    if (transactableBag.getDiscountTotal() > 0) {
      rithumOrder.setAdditionalCostOrDiscount(-PriceUtils.centsToDollars(transactableBag.getDiscountTotal()));
    }

    // Apply components with focused helper methods
    applyLineItems(transactableBag, rithumOrder);
    applyCustomerInfo(transactableBag, rithumOrder);
    applyShippingAddress(transactableBag, rithumOrder);
    applyBillingAddress(transactableBag, rithumOrder);
    applyStatusMappings(transactableBag, rithumOrder);

    return rithumOrder;
  }
}
```

### Product Decomposer (Advanced Patterns)

The `RithumProductDecomposer` handles complex product structures:

```java
public class RithumProductDecomposer {
  public static Offer decompose(RithumProduct rithumProduct, Merchant merchant, RithumOfferSyncConfig config) {
    // Multi-product type handling
    if (RithumProductType.VARIATION_PARENT.equals(rithumProduct.getProductType())) {
      return decomposeVariationParent(rithumProduct, merchant, config);
    } else {
      return decomposeStandardProduct(rithumProduct, merchant, config);
    }
  }

  private static Offer decomposeVariationParent(RithumProduct product, Merchant merchant, RithumOfferSyncConfig config) {
    Offer offer = new Offer();

    // Extract variants from child product attributes
    populateOfferVariants(product, offer);
    // Map each child product to individual SKU with variant values
    populateOfferSkus(product, offer, merchant);

    return offer;
  }
}
```

### Advanced Status Mapping

Rithum uses a three-tier status system:

```java
private static RithumCheckoutStatus mapBagStatusToCheckoutStatus(Bag.Status status) {
  return switch (status) {
    case IN_PROGRESS, SUBMITTED, ACCEPTED -> RithumCheckoutStatus.COMPLETED;
    case CANCELED -> RithumCheckoutStatus.CANCELLED_BY_SELLER;
    case REJECTED -> RithumCheckoutStatus.CANCELLED_BY_CHANNEL;
    case BACKORDERED -> RithumCheckoutStatus.ON_HOLD;
    default -> RithumCheckoutStatus.PENDING;
  };
}

private static RithumPaymentStatus mapBagStatusToPaymentStatus(Bag.Status status) {
  return switch (status) {
    case ACCEPTED, COMPLETED -> RithumPaymentStatus.PAID;
    case CANCELED, REJECTED -> RithumPaymentStatus.CANCELLED;
    default -> RithumPaymentStatus.PENDING;
  };
}

private static RithumShippingStatus mapBagStatusToShippingStatus(Bag.Status status) {
  return switch (status) {
    case COMPLETED -> RithumShippingStatus.SHIPPED;
    case CANCELED, REJECTED -> RithumShippingStatus.CANCELLED;
    default -> RithumShippingStatus.PENDING_SHIPMENT;
  };
}
```

### Fallback Shipping Methods

Rithum supports configurable fallback shipping methods:

```java
public boolean shouldUseFallbackShippingMethods(MerchantConfiguration config) {
  return RithumMerchantConfig.shouldUseFallbackShippingMethods(config);
}

// In order sync service
if (shouldUseFallbackShippingMethods(merchantConfig)) {
  applyFallbackShippingOptions(order, availableShippingMethods);
}
```

## Testing Patterns

### Static Method Mocking

Rithum requires extensive static method mocking:

```java
@Test
public void testOrderSyncWithFallbackShipping() {
  try (MockedStatic<RithumMerchantConfig> mockedRithumConfig = Mockito.mockStatic(RithumMerchantConfig.class);
       MockedStatic<CartCalculationUtils> mockedUtils = Mockito.mockStatic(CartCalculationUtils.class)) {

    // Setup static method mocks
    mockedRithumConfig.when(() -> RithumMerchantConfig.shouldUseFallbackShippingMethods(config)).thenReturn(true);

    // Execute test
    List<OfferData.Shipping> result = rithumOrderSyncService.getShippingEstimates(request);

    // Verify results
    assertNotNull(result);
    assertTrue(CollectionUtils.isNotEmpty(result));
  }
}
```

### MerchantConfiguration Setup

```java
private MerchantConfiguration createMerchantConfiguration(boolean useFallbackShipping) {
  MerchantConfiguration config = new MerchantConfiguration();
  RithumMerchantConfig rithumConfig = new RithumMerchantConfig();
  rithumConfig.setUseFallbackShippingMethods(useFallbackShipping);
  config.setRithumConfig(rithumConfig);  // NOT setRithum()
  return config;
}
```

### Test Data Patterns

```java
private DecryptedMerchantCredentials createDecryptedMerchantCredentials() {
  DecryptedMerchantCredentials dmc = new DecryptedMerchantCredentials();
  dmc.setMerchantId(12345);  // Use Integer.valueOf(), not Long
  dmc.setPlatform(Merchant.Platform.RITHUM);
  return dmc;
}
```

## Common Issues & Solutions

### Compilation Issues Fixed

**OfferData.Shipping.Price Structure**:
```java
// WRONG
OfferData.Shipping.Method.Price price = new OfferData.Shipping.Method.Price();

// CORRECT
OfferData.Shipping.Price price = new OfferData.Shipping.Price();
```

**MerchantConfiguration Method Names**:
```java
// WRONG - common mistake when working with Rithum
config.setRithum(rithumConfig);

// CORRECT - the actual method name
config.setRithumConfig(rithumConfig);
```

**MockBean with Qualifiers**:
```java
// PROBLEMATIC
@MockBean
@Qualifier("internal")
private Gson internalGson;

// BETTER
@MockBean(name = "internal")
private Gson internalGson;
```

**Type Consistency in Assertions**:
```java
// WRONG
assertEquals(Long.valueOf(12345L), result.getMerchantId());

// CORRECT
assertEquals(Integer.valueOf(12345), result.getMerchantId());
```

## Platform-Specific Considerations

### Product Type Handling
- **VARIATION_PARENT**: Products with child variants
- **STANDARD**: Single products without variants
- **DIGITAL**: Digital/downloadable products
- **BUNDLE**: Product bundles and kits

### Discount Handling
- Rithum uses negative `additionalCostOrDiscount` for discount amounts
- Handle both percentage and fixed amount discounts
- Apply discounts at order level, not line item level

### Address Mapping
- Rithum provides separate billing and shipping address fields
- Direct property mapping without complex constructors
- Handle missing address components gracefully

### Order Item Structure
- Simple structure: SKU, title, quantity, unit price
- No complex variant option mapping required
- Direct external ID usage without splitting

## Best Practices

### Leverage Data Model Guarantees
```java
// DON'T: Add unnecessary null checks for guaranteed non-null fields
if (Objects.nonNull(transactableBag.getTotal())) {
  order.setTotalPrice(PriceUtils.centsToDollars(transactableBag.getTotal()));
}

// DO: Trust the data model guarantees
order.setTotalPrice(PriceUtils.centsToDollars(transactableBag.getTotal()));
```

### Clean Separation of Concerns
- Keep main composer methods focused on orchestration
- Extract complex logic into focused helper methods
- Use static utility classes for reusable functionality

### Comprehensive Testing Strategy
- Test successful composition with complete data
- Test edge cases: null addresses, empty collections, various statuses
- Test status mapping for all enum values
- Test fallback shipping method configuration
- Verify property mapping accuracy with assertions
- Use realistic test data matching production scenarios

## Migration Guidelines

When working with existing Rithum implementations:

1. **Identify Static Method Dependencies**: Look for `RithumMerchantConfig` usage
2. **Setup Proper Mocking**: Use try-with-resources for `MockedStatic`
3. **Verify Configuration Setup**: Use correct setter methods
4. **Check Return Types**: Ensure assertion types match actual return types
5. **Test Data Completeness**: Create realistic object graphs with proper relationships

This platform requires careful attention to static method mocking and configuration setup patterns.

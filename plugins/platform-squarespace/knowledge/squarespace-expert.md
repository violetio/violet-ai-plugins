# Squarespace Platform - CLAUDE.md

Platform-specific guidance for Squarespace integration development.

## Platform Overview

Squarespace is a website building and ecommerce platform that combines content management with commerce capabilities. It offers both online store and POS integration with comprehensive inventory management.

### Key Features
- **Unified Platform**: Website builder and ecommerce in one platform
- **POS Integration**: Physical and online sales integration
- **Inventory Management**: Real-time inventory tracking across channels
- **Design Focus**: Template-based design system with commerce integration
- **Commerce API**: REST API for ecommerce operations
- **Order Management**: Comprehensive order lifecycle management
- **Tax Integration**: Built-in tax calculation with third-party integrations
- **Member Areas**: Customer account and membership functionality

## Architecture Overview

### Core Services
- `SquarespaceOrderSyncService`: Order synchronization and management
- `SquarespaceProductSyncService`: Product catalog operations
- `SquarespaceShopSyncService`: Shop configuration and settings
- `SquarespaceServiceClientFactory`: Authentication and API client management
- `SquarespaceWebhookService`: Webhook event processing

### Key Components

**Composers (Violet → Squarespace)**
- `SquarespaceOrderComposer`: Order creation with currency objects
- `SquarespaceProductComposer`: Product creation and updates

**Decomposers (Squarespace → Violet)**
- `SquarespaceProductDecomposer`: Product data transformation (instance-based)
- `SquarespaceOrderDecomposer`: Order data transformation

**Utilities**
- `SquarespaceOrderComposerUtils`: Order composition helper methods
- `SquarespaceOrderDecomposerUtils`: Order decomposition utilities
- `SquarespaceOrderUtils`: Order processing utilities
- `SquarespaceProductUtils`: Product management utilities
- `SquarespaceShopUtils`: Shop configuration utilities
- `SquarespaceUtils`: General platform utilities
- `MerchantCredentialMapper`: Credential mapping utilities

## Development Patterns

### Order Composer (Currency Objects Pattern)

Squarespace requires platform-specific currency amount objects:

```java
public class SquarespaceOrderComposer {
  public static SquarespaceOrder composeOrder(TransactableBag transactableBag, MerchantConfiguration merchantConfig) {
    SquarespaceOrder sqOrder = new SquarespaceOrder();

    // Platform-specific currency amount objects for all pricing fields
    applyCurrencyObjects(transactableBag, sqOrder);

    // Contextual pricing with currency conversion for line items
    applyContextualPricing(transactableBag, sqOrder, merchantConfig);

    // Fulfillment mapping with detailed status transitions
    applyFulfillmentMapping(transactableBag, sqOrder);

    // Refund amount tracking with separate currency handling
    applyRefundTracking(transactableBag, sqOrder);

    return sqOrder;
  }
}
```

### Currency Object Handling

```java
private static void applyCurrencyObjects(TransactableBag bag, SquarespaceOrder order) {
  // Create currency objects for all monetary values
  SquarespaceMoney totalMoney = new SquarespaceMoney();
  totalMoney.setCurrency(bag.getCurrency());
  totalMoney.setValue(PriceUtils.centsToDollars(bag.getTotal()));
  order.setGrandTotal(totalMoney);

  SquarespaceMoney subtotalMoney = new SquarespaceMoney();
  subtotalMoney.setCurrency(bag.getCurrency());
  subtotalMoney.setValue(PriceUtils.centsToDollars(bag.getSubtotal()));
  order.setSubtotal(subtotalMoney);

  SquarespaceMoney taxMoney = new SquarespaceMoney();
  taxMoney.setCurrency(bag.getCurrency());
  taxMoney.setValue(PriceUtils.centsToDollars(bag.getTaxTotal()));
  order.setTaxTotal(taxMoney);

  SquarespaceMoney shippingMoney = new SquarespaceMoney();
  shippingMoney.setCurrency(bag.getCurrency());
  shippingMoney.setValue(PriceUtils.centsToDollars(bag.getShippingTotal()));
  order.setShippingTotal(shippingMoney);
}
```

### Contextual Pricing with Currency Conversion

```java
private static void applyContextualPricing(TransactableBag bag, SquarespaceOrder order, MerchantConfiguration config) {
  if (MerchantConfiguration.shouldUseContextualPrices(config)) {
    // Apply currency conversion for line items
    for (OrderSku orderSku : bag.getSkus()) {
      SquarespaceLineItem lineItem = new SquarespaceLineItem();

      // Convert price using exchange rate if available
      BigDecimal price = PriceUtils.centsToDollars(orderSku.getPrice());
      if (Objects.nonNull(bag.getCurrencyExchangeRate())) {
        price = price.multiply(bag.getCurrencyExchangeRate());
      }

      SquarespaceMoney lineItemPrice = new SquarespaceMoney();
      lineItemPrice.setCurrency(bag.getCurrency());
      lineItemPrice.setValue(price);
      lineItem.setUnitPrice(lineItemPrice);

      order.getLineItems().add(lineItem);
    }
  }
}
```

### Fulfillment Status Mapping

```java
private static void applyFulfillmentMapping(TransactableBag bag, SquarespaceOrder order) {
  SquarespaceFulfillmentStatus fulfillmentStatus = mapBagStatusToFulfillmentStatus(bag.getStatus());
  order.setFulfillmentStatus(fulfillmentStatus);

  // Detailed status transitions
  switch (bag.getStatus()) {
    case IN_PROGRESS:
      order.setOrderStatus(SquarespaceOrderStatus.PENDING);
      order.setPaymentStatus(SquarespacePaymentStatus.PENDING);
      break;
    case ACCEPTED:
      order.setOrderStatus(SquarespaceOrderStatus.CONFIRMED);
      order.setPaymentStatus(SquarespacePaymentStatus.PAID);
      break;
    case COMPLETED:
      order.setOrderStatus(SquarespaceOrderStatus.FULFILLED);
      order.setPaymentStatus(SquarespacePaymentStatus.PAID);
      fulfillmentStatus = SquarespaceFulfillmentStatus.SHIPPED;
      break;
    case CANCELED:
      order.setOrderStatus(SquarespaceOrderStatus.CANCELED);
      order.setPaymentStatus(SquarespacePaymentStatus.REFUNDED);
      break;
  }
}
```

### Refund Amount Tracking

```java
private static void applyRefundTracking(TransactableBag bag, SquarespaceOrder order) {
  if (bag.getRefundTotal() > 0) {
    SquarespaceMoney refundMoney = new SquarespaceMoney();
    refundMoney.setCurrency(bag.getCurrency());
    refundMoney.setValue(PriceUtils.centsToDollars(bag.getRefundTotal()));

    SquarespaceRefund refund = new SquarespaceRefund();
    refund.setAmount(refundMoney);
    refund.setReason("Customer request");
    refund.setStatus(SquarespaceRefundStatus.PROCESSED);

    order.getRefunds().add(refund);
  }
}
```

### Product Decomposer (Instance Architecture)

**Note**: Squarespace uses instance architecture but is not considered legacy:

```java
public class SquarespaceProductDecomposer {
  // Instance architecture but not legacy - acceptable pattern for Squarespace
  private final SquarespaceProductUtils productUtils;

  public Offer decompose(SquarespaceProduct product, Merchant merchant) {
    Offer offer = new Offer();

    // Handle Squarespace-specific product structure
    populateBasicInfo(product, offer, merchant);
    populateVariants(product, offer);
    populateInventory(product, offer);
    populateImages(product, offer);

    return offer;
  }

  private void populateInventory(SquarespaceProduct product, Offer offer) {
    // Squarespace inventory tracking
    if (product.getVariants() != null) {
      for (SquarespaceVariant variant : product.getVariants()) {
        Sku sku = new Sku();
        sku.setQtyAvailable(variant.getStock());
        sku.setInfiniteQuantity(variant.isUnlimitedStock());
        offer.getSkus().add(sku);
      }
    }
  }
}
```

## Testing Patterns

### Test Structure
- Tests located in `src/test/java/io/drizzl/platform/ecom/platforms/squarespace/`
- Use `@SpringBootTest` with Squarespace-specific mocks
- JSON fixtures in `src/test/resources/platform/squarespace/`

### Mock Examples
```java
@MockBean
private SquarespaceServiceClientFactory sqServiceClientFactory;

@MockBean
private SquarespaceOrderComposerUtils orderComposerUtils;

// Mock currency conversion
when(merchantConfiguration.shouldUseContextualPrices()).thenReturn(true);
```

### Test Data
- Product structures with Squarespace-specific fields
- Order responses with currency objects
- Inventory data with stock tracking

## Common Issues & Solutions

### Currency Object Complexity
- All monetary values must use `SquarespaceMoney` objects
- Consistent currency handling across all price fields
- Proper decimal precision for currency calculations

### Design Integration
- Products must consider Squarespace's design system
- Image sizing and optimization requirements
- Template compatibility considerations

### POS Integration
```java
// Handle POS vs online order differentiation
if (SquarespaceOrderUtils.isPosOrder(order)) {
  // Apply POS-specific processing
  applyPosOrderHandling(order);
} else {
  // Apply online order processing
  applyOnlineOrderHandling(order);
}
```

### Inventory Synchronization
```java
// Sync inventory across POS and online channels
private void syncInventoryAcrossChannels(SquarespaceProduct product) {
  for (SquarespaceVariant variant : product.getVariants()) {
    // Update both POS and online inventory
    updatePosInventory(variant);
    updateOnlineInventory(variant);
  }
}
```

## Platform-Specific Considerations

### Website Integration
- Products are integrated into website design templates
- SEO considerations for product pages
- Content management integration

### Member Areas
- Customer account functionality
- Subscription and membership products
- Access control for digital products

### Tax Interpretation Flags
```java
// Squarespace supports tax interpretation flags
order.setTaxInterpretation(SquarespaceTaxInterpretation.INCLUSIVE);
// or
order.setTaxInterpretation(SquarespaceTaxInterpretation.EXCLUSIVE);
```

### Design System Integration
- Product images must fit design templates
- Color and styling consistency
- Responsive design considerations

### Analytics Integration
- Built-in analytics for ecommerce
- Custom tracking requirements
- Conversion funnel analysis

## Migration Guidelines

### API Version Updates
- Stay current with Squarespace Commerce API versions
- Handle breaking changes in currency object structure
- Test with different template configurations

### Design System Changes
- Monitor template updates that affect commerce
- Test commerce functionality with design changes
- Ensure product display consistency

## Debugging Tools

### Commerce API Testing
```java
// Test Squarespace Commerce API endpoints
String apiUrl = "https://api.squarespace.com/1.0/commerce/orders";
// Include proper authentication and currency objects
```

### Template Testing
- Test product display in different templates
- Verify checkout flow in various designs
- Check mobile responsiveness

### POS Integration Testing
- Test inventory sync between POS and online
- Verify order processing from both channels
- Check customer data synchronization

This platform requires careful attention to currency object handling and design system integration considerations.

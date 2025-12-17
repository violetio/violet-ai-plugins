# BigCommerce Development Patterns

## Order Composer (Legacy Instance Architecture)

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

## Product Decomposer (Legacy Pattern)

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

## Product Option Mapping

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

## Coupon and Discount Processing

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

## Extensive Metadata System

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

## Numeric Status ID Mapping

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

## Webhook Signature Validation

```java
// Validate BigCommerce webhook signatures
String expectedSignature = BigCommerceWebhookUtils.calculateSignature(payload, webhookSecret);
if (!BigCommerceWebhookUtils.isValidSignature(receivedSignature, expectedSignature)) {
  throw new InvalidWebhookSignatureException();
}
```

## Rate Limiting Strategy

- BigCommerce has strict rate limits: 450 requests per 30 seconds
- Implement exponential backoff with jitter
- Monitor `X-Rate-Limit-Remaining` headers
- Consider using message queues for bulk operations
- Test with actual payload sizes to measure request impact

## Authentication

```java
// BigCommerce authentication setup
// Store Hash: Required for API authentication
// Access Token: OAuth token management
// API Path: Correct API version and path construction

String apiUrl = "https://api.bigcommerce.com/stores/{store_hash}/v3/...";
// Include authentication headers with access token
```

## Migration from Legacy to Modern

Target modern pattern for BigCommerce:

```java
// Target modern pattern
public class BigcommerceOrderComposer {
  public static BigCommerceOrder composeOrder(TransactableBag bag, MerchantConfiguration config) {
    // Static method implementation
    // Use utility methods for complex logic
    // Remove instance variable dependencies
  }
}
```

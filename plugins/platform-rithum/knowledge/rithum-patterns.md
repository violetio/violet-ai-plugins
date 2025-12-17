# Rithum Development Patterns

## Modern Static Order Composer

```java
public class RithumOrderComposer {
  public static RithumOrder composeOrder(TransactableBag transactableBag, MerchantConfiguration merchantConfig) {
    RithumOrder rithumOrder = new RithumOrder();

    // Direct property assignment
    rithumOrder.setTotalPrice(PriceUtils.centsToDollars(transactableBag.getTotal()));
    rithumOrder.setTotalTax(PriceUtils.centsToDollars(transactableBag.getTaxTotal()));
    rithumOrder.setShippingTotal(PriceUtils.centsToDollars(transactableBag.getShippingTotal()));

    // Discounts as negative adjustments
    if (transactableBag.getDiscountTotal() > 0) {
      rithumOrder.setAdditionalCostOrDiscount(-PriceUtils.centsToDollars(transactableBag.getDiscountTotal()));
    }

    applyLineItems(transactableBag, rithumOrder);
    applyCustomerInfo(transactableBag, rithumOrder);
    applyStatusMappings(transactableBag, rithumOrder);

    return rithumOrder;
  }
}
```

## Three-Tier Status Mapping

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
```

## Product Decomposer (Variation Parents)

```java
public class RithumProductDecomposer {
  public static Offer decompose(RithumProduct rithumProduct, Merchant merchant, RithumOfferSyncConfig config) {
    if (RithumProductType.VARIATION_PARENT.equals(rithumProduct.getProductType())) {
      return decomposeVariationParent(rithumProduct, merchant, config);
    } else {
      return decomposeStandardProduct(rithumProduct, merchant, config);
    }
  }

  private static Offer decomposeVariationParent(RithumProduct product, Merchant merchant, RithumOfferSyncConfig config) {
    Offer offer = new Offer();
    populateOfferVariants(product, offer);
    populateOfferSkus(product, offer, merchant);
    return offer;
  }
}
```

## Fallback Shipping Methods

```java
public boolean shouldUseFallbackShippingMethods(MerchantConfiguration config) {
  return RithumMerchantConfig.shouldUseFallbackShippingMethods(config);
}

if (shouldUseFallbackShippingMethods(merchantConfig)) {
  applyFallbackShippingOptions(order, availableShippingMethods);
}
```

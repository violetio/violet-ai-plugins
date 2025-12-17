# Squarespace Development Patterns

## Currency Object Handling

Squarespace requires platform-specific currency objects:

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
}
```

## Contextual Pricing with Currency Conversion

```java
private static void applyContextualPricing(TransactableBag bag, SquarespaceOrder order, MerchantConfiguration config) {
  if (MerchantConfiguration.shouldUseContextualPrices(config)) {
    for (OrderSku orderSku : bag.getSkus()) {
      SquarespaceLineItem lineItem = new SquarespaceLineItem();

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

## Fulfillment Status Mapping

```java
private static void applyFulfillmentMapping(TransactableBag bag, SquarespaceOrder order) {
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
      order.setFulfillmentStatus(SquarespaceFulfillmentStatus.SHIPPED);
      break;
    case CANCELED:
      order.setOrderStatus(SquarespaceOrderStatus.CANCELED);
      order.setPaymentStatus(SquarespacePaymentStatus.REFUNDED);
      break;
  }
}
```

## Refund Amount Tracking

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

## Product Inventory Management

```java
private void populateInventory(SquarespaceProduct product, Offer offer) {
  if (product.getVariants() != null) {
    for (SquarespaceVariant variant : product.getVariants()) {
      Sku sku = new Sku();
      sku.setQtyAvailable(variant.getStock());
      sku.setInfiniteQuantity(variant.isUnlimitedStock());
      offer.getSkus().add(sku);
    }
  }
}
```

## POS vs Online Order Differentiation

```java
// Handle POS vs online order differentiation
if (SquarespaceOrderUtils.isPosOrder(order)) {
  applyPosOrderHandling(order);
} else {
  applyOnlineOrderHandling(order);
}
```

# WooCommerce Development Patterns

## Metadata Patterns

WooCommerce extensively uses metadata:

```java
private void applyMetadata(TransactableBag bag, WooCommerceOrder order) {
  order.addMetaData("violet_order_id", bag.getOrderId());
  order.addMetaData("violet_bag_id", bag.getId());

  if (StringUtils.isNotEmpty(bag.getTrackingNumber())) {
    order.addMetaData("tracking_number", bag.getTrackingNumber());
  }
}
```

## Line-Item Discount Distribution

```java
private void applyLineItemDiscounts(TransactableBag bag, WooCommerceOrder order) {
  final Map<Long, Double> lineItemDiscounts = OrderUtils.getPerLineItemDiscountAmounts(bag);

  for (OrderSku orderSku : bag.getSkus()) {
    WooCommerceLineItem lineItem = new WooCommerceLineItem();
    int skuDiscount = lineItemDiscounts.containsKey(orderSku.getId())
        ? PriceUtils.dollarsToCents(lineItemDiscounts.get(orderSku.getId()))
        : 0;
    int lineItemTotal = orderSku.getLinePrice() - skuDiscount;
    lineItem.setTotal(PriceUtils.centsToDollars(lineItemTotal));
    order.getLineItems().add(lineItem);
  }
}
```

## Product Variation Handling

```java
if ("variable".equals(product.getType())) {
  for (WooCommerceVariation variation : product.getVariations()) {
    Sku sku = createSkuFromVariation(variation, merchant);
    offer.getSkus().add(sku);
  }
} else {
  Sku sku = createSkuFromProduct(product, merchant);
  offer.getSkus().add(sku);
}
```

## Digital vs Physical Orders

```java
// Skip shipping for digital-only orders
if (!OrderUtils.allDigitalItems(transactableBag.getSkus())) {
  applyShippingAddress(transactableBag, wooOrder);
}
```

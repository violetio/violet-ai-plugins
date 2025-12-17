# Shopify Development Patterns

## GraphQL Order Composer (Modern Approach)

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

## Product Decomposer (Modern Static)

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

## GraphQL Query Building

```java
// Use ShopifyGqlQueries for consistent query construction
String productQuery = ShopifyGqlQueries.getProductByIdQuery(productId, includeVariants);
ShopifyGraphqlResponse response = shopifyGraphUtils.executeQuery(query, variables);
```

## Product Composer (Modern Static)

The `ShopifyProductComposer` transforms Violet Offers into Shopify Products:

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

## Key Composition Patterns

**Product Status Mapping**
- `Offer.Status.ARCHIVED` → `Product.Status.archived`
- Availability + visibility → `active`

**Variant Mapping**
- Violet SKUs → Shopify Variants
- Proper option1/option2/option3 assignment

**Media Transformation**
- Violet Albums → Shopify Images
- Position and external ID preservation

**Metafield Conversion**
- Violet MetaObjects → Shopify Metafields
- Type mapping and namespace management

**Pricing Conversion**
- Use `PriceUtils.centsToDollars()` for price conversion
- Handle decimal precision

**Inventory Management**
- Map `inventoryTracked` boolean to `inventory_policy`
- "deny" for tracked, "continue" for untracked

## Product Type Handling

```java
private static Boolean determineRequiresShipping(Product.Type productType) {
  return switch (productType) {
    case DIGITAL -> false;
    case PHYSICAL, BUNDLED -> true;
    default -> true;
  };
}
```

## Variant Option Mapping

Shopify supports max 3 options - map SKU variant values to option1/option2/option3:

```java
switch (optionIndex) {
  case 0 -> shopifyVariant.setOption1(optionValue);
  case 1 -> shopifyVariant.setOption2(optionValue);
  case 2 -> shopifyVariant.setOption3(optionValue);
}
```

## Exchange Rate Handling

```java
// Shopify supports presentment currencies - handle exchange rates
if (Objects.nonNull(transactableBag.getCurrencyExchangeRate())) {
  BigDecimal invertedRate = OrderUtils.invertExchangeRate(transactableBag.getCurrencyExchangeRate());
  ShopifyGraphOrderComposerUtils.applyExchangeRate(draftOrderInput, invertedRate);
}
```

## Inventory Behavior Settings

```java
// Configure inventory behavior for GraphQL orders
draftOrderInput.setInventoryBehaviour(InventoryBehaviour.DECREMENT_IGNORING_POLICY);
```

## Error Handling

```java
try {
  // Shopify API call
} catch (ShopifyApiException e) {
  ShopifyErrorUtils.handleApiError(e, merchantId);
  throw new PlatformSyncException("Shopify sync failed", e);
}
```

## Migration Patterns

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

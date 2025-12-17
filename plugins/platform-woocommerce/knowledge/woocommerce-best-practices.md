# WooCommerce Best Practices

## WordPress Plugin Dependency

- WooCommerce plugin must be installed and activated
- Verify API version compatibility
- Check for plugin conflicts
- Test with minimal plugin setup

## Authentication

OAuth 1.0a with Consumer Key/Secret:
- HTTPS required for secure transmission
- Verify read/write permissions
- Plan for periodic key rotation

## Metadata Management

- Consistent key naming conventions
- Proper serialization for complex objects
- Documentation of custom fields

## Product Variations

Handle complex parent-child relationships:
- Stock tracking per variation
- Image associations
- Price variations and modifiers

## Tax and Shipping

- Complex tax calculation with multiple rates
- Flexible shipping zone configuration
- Test tax-inclusive vs tax-exclusive pricing

## Performance

- Implement pagination for bulk operations
- Use filtering to reduce payload size
- Process webhooks asynchronously
- Monitor WordPress database performance

## Migration Guidelines

Legacy instance-based composers need migration to static methods:

```java
// Target pattern
public class WooCommerceOrderComposer {
  public static WooCommerceOrder composeOrder(TransactableBag bag, MerchantConfiguration config) {
    // Static implementation
  }
}
```

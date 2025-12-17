# Shopify Platform Overview

## Platform Summary

Shopify is a leading ecommerce platform with comprehensive GraphQL and REST API support. This integration supports both public and custom app installations with extensive webhook capabilities.

### Key Features
- **GraphQL API**: Primary API for modern integrations with complex queries
- **REST API**: Legacy support for specific operations
- **Custom App Scopes**: Granular permission system
- **Storefront API**: Customer-facing operations
- **Webhook System**: Real-time event notifications
- **Multi-channel Support**: Online store, POS, social commerce

## Core Services

### Order Processing
- `ShopifyOrderSyncService`: Order synchronization and management
- `ShopifyCustomerSyncService`: Customer data management
- `ShopifyShopSyncService`: Shop configuration and settings

### Product Management
- `ShopifyProductSyncService`: Product catalog operations
- `ShopifyWebhookService`: Webhook event processing

### API & Authentication
- `ShopifyServiceClientFactory`: Authentication and API client management

## Composers (Violet → Shopify)

### Modern Approach
- **`ShopifyGraphOrderComposer`**: GraphQL-based order creation (RECOMMENDED)
- **`ShopifyProductComposer`**: Static product composition for creation/updates

### Legacy Approach
- `ShopifyOrderComposer`: REST API order creation
- `ShopifyDraftOrderComposer`: Draft order management
- `ShopifyStorefrontCartComposer`: Customer cart operations

## Decomposers (Shopify → Violet)

### Modern Approach
- **`ShopifyGraphProductDecomposer`**: Static product decomposition (RECOMMENDED)

### Legacy Approach
- `ShopifyProductDecomposer`: Legacy product decomposition
- `ShopifyOrderDecomposer`: Order data transformation
- `ShopifyCustomerDecomposer`: Customer data transformation
- `ShopifyCollectionDecomposer`: Product collection handling

## Utilities

**GraphQL**
- `ShopifyGraphUtils`: GraphQL query building and execution
- `ShopifyGraphOrderComposerUtils`: Order composition helper methods

**Core**
- `ShopifyAuthUtils`: Authentication and token management
- `ShopifyErrorUtils`: Error handling and retry logic
- `ShopifyWebhookUtils`: Webhook signature validation

## Architectural Patterns

### API Strategy
- **Prefer GraphQL** for new implementations (better performance, type safety)
- **Use REST** only for operations not supported in GraphQL
- **Migrate legacy REST** implementations to GraphQL when possible

### Rate Limiting
- GraphQL: 1000 points/sec
- REST: 40 calls/sec
- Use exponential backoff with jitter
- Monitor rate limit headers in responses

### Multi-Location Inventory
- Shopify supports multiple inventory locations
- Handle location-specific stock quantities
- Consider fulfillment location preferences

### Product Variants
- Shopify has complex variant structures (options + values)
- Maximum 3 options per product
- Maximum 100 variants per product
- Careful mapping to Violet's variant system

### Metafields
- Extensive metafield system for custom data
- Use for storing Violet-specific references
- Namespace metafields properly (`violet.offer_id`, etc.)

## Critical Implementation Patterns

### Status Mapping
- Map Violet Order statuses to Shopify order statuses
- Handle fulfillment status transitions
- Support for payment status tracking

### Currency Support
- Shopify supports presentment currencies
- Handle exchange rates for multi-currency orders
- Convert prices using `PriceUtils.centsToDollars()`

### Scope Requirements
```java
// Check required scopes before API calls
List<String> requiredScopes = Arrays.asList("read_products", "write_orders");
if (!ShopifyScopeUtils.hasRequiredScopes(merchant, requiredScopes)) {
  throw new InsufficientScopesException();
}
```

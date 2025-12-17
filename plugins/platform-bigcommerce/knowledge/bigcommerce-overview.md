# BigCommerce Platform Overview

## Platform Summary

BigCommerce is an enterprise-focused ecommerce platform with comprehensive REST API support and advanced webhook capabilities. The platform is designed for larger merchants with complex requirements.

### Key Features
- **Enterprise Focus**: Built for high-volume, complex commerce operations
- **REST API**: Comprehensive REST API with extensive documentation
- **Webhook System**: Real-time event notifications with retry logic
- **Product Variants**: Complex product option and variant system
- **Multi-Channel**: Headless commerce and API-first architecture
- **Advanced Catalog**: Complex product relationships and categories
- **Extensive Metadata**: Rich product and order metadata system
- **Payment Gateways**: Support for multiple payment providers

## Core Services

### Order Processing
- `BigCommerceOrderSyncService`: Order synchronization and management
- `BigCommerceCustomerSyncService`: Customer data management
- `BigCommerceShippingSyncService`: Shipping method and zone management

### Product Management
- `BigCommerceProductSyncService`: Product catalog operations
- `BigCommerceShopSyncService`: Shop configuration and settings

### API & Events
- `BigCommerceServiceClientFactory`: Authentication and API client management
- `BigcommerceWebhookSyncService`: Webhook event processing
- `AsyncBcOrderEvents`: Asynchronous order event processing

## Composers (Violet → BigCommerce)

### Order Composition
- `BigcommerceOrderComposer`: Order composition (NOTE: Uses legacy instance architecture, needs modernization)

## Decomposers (BigCommerce → Violet)

### Product Decomposition
- `BigCommerceProductDecomposer`: Product decomposition (NOTE: Uses legacy instance architecture)

### Order & Customer Decomposition
- `BigCommerceOrderDecomposer`: Order data transformation
- `BigCommerceCustomerDecomposer`: Customer data transformation

## Utilities

**Calculations**
- `BigCommerceCalculateCartUtils`: Cart and pricing calculations
- `BigCommerceDiscountUtils`: Discount and coupon utilities
- `BigCommerceShippingRelayUtils`: Shipping calculation helpers

**Processing**
- `BigCommerceCartUtils`: Cart management utilities
- `BigCommerceCommonUtils`: Shared utility functions
- `BigCommerceOrderUtils`: Order processing utilities
- `BigCommerceOrderDecomposerUtils`: Order decomposition helpers
- `BigCommerceProductDecomposerUtils`: Product decomposition utilities
- `BigCommerceProductSyncUtils`: Product synchronization utilities
- `BigCommerceWebhookUtils`: Webhook processing utilities

## Architectural Characteristics

### Legacy Pattern
**Status**: BigCommerce uses legacy instance-based architecture
- Instance-based composers and decomposers
- Multiple instance variables for utilities
- Should be migrated to modern static patterns
- See Migration Guidelines section

### Product Option System
- Complex option/variant relationships
- Multiple option types: dropdown, radio, checkbox
- Option value dependencies
- Pricing modifiers per option value

### Numeric Status IDs
BigCommerce uses numeric IDs instead of string statuses:
- 1: Pending
- 2: Awaiting Shipment
- 5: Cancelled
- 6: Declined
- 10: Completed
- 11: Awaiting Fulfillment

### Metadata System
- Order-level custom fields
- Line item-level custom fields
- Customer metadata
- Extensible for Violet-specific data

## Critical Implementation Patterns

### Coupon and Discount Processing
- Support for coupon codes
- Discount amount application
- Support for multiple discounts per order
- Fixed amount or percentage-based discounts

### Product Option Mapping
- Complex mapping from BigCommerce options to Violet variants
- Option display names to variant names
- Option values to variant values
- Pricing modifiers per option

### Cart Calculation
- Cart totals with discounts
- Shipping cost calculation
- Tax calculation
- Multi-currency support

### Webhook Integration
- Webhook signature validation required
- Retry logic with exponential backoff
- Topic-based event subscriptions
- Event payload processing

## Rate Limiting

- **Strict Rate Limits**: 450 requests per 30 seconds
- **Exponential Backoff**: Implement with jitter
- **Header Monitoring**: Watch `X-Rate-Limit-Remaining` headers
- **Queue Management**: Consider using message queues for bulk operations

## Authentication Requirements

- **Store Hash**: Required for API authentication
- **Access Token**: OAuth token management
- **API Path**: Correct API version and path construction
- **HTTPS**: All API calls must use HTTPS

## Platform-Specific Features

### Enterprise Capabilities
- Advanced catalog management with categories and brands
- Customer groups and pricing tiers
- Multi-channel inventory management
- Advanced shipping and tax rules

### Order Management
- Comprehensive order lifecycle management
- Advanced fulfillment options
- Return and exchange processing
- Order notes and communications

### Payment Processing
- Multiple payment gateway support
- Stored payment methods
- Recurring billing capabilities
- Fraud detection integration

### Multi-Channel Support
- Headless commerce capabilities
- API-first architecture
- Channel-specific configurations
- Cross-channel inventory sync

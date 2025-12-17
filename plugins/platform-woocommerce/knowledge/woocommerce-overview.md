# WooCommerce Platform Overview

## Platform Summary

WooCommerce is a WordPress-based ecommerce platform that powers millions of online stores. The integration requires the WooCommerce plugin to be installed and properly configured on the WordPress site.

### Key Features
- **WordPress Integration**: Built on WordPress with extensive plugin ecosystem
- **REST API**: Comprehensive REST API for all ecommerce operations
- **Metadata System**: Extensive metadata support for custom properties
- **Product Variations**: Complex product variation system
- **Coupon System**: Advanced discount and coupon management
- **Shipping Zones**: Flexible shipping configuration
- **Tax Management**: Comprehensive tax calculation system

## Core Services

- `WooCommerceOrderSyncService`: Order synchronization and management
- `WooCommerceProductSyncService`: Product catalog operations
- `WooCommerceCustomerSyncService`: Customer data management
- `WooCommerceTaxSyncService`: Tax rate and configuration management
- `WooCommerceShippingSyncService`: Shipping method and zone management
- `WooCommerceServiceClientFactory`: Authentication and API client management
- `WooCommerceWebhookService`: Webhook event processing

## Composers & Decomposers

**Composers**: `WooCommerceOrderComposer` (legacy instance-based)
**Decomposers**: `WooCommerceProductDecomposer`, `WooProductV3Decomposer`, `WooCommerceOrderDecomposer`

## Key Patterns

### Metadata-Heavy Design
- Order-level metadata for custom properties
- Line item metadata for tracking
- Customer metadata for preferences

### Product Types
- Simple, Variable, Grouped, External, Virtual, Downloadable

### Authentication
OAuth 1.0a with Consumer Key/Secret (HTTPS required)

# Squarespace Platform Overview

## Platform Summary

Squarespace is a website building and ecommerce platform that combines content management with commerce capabilities. It offers both online store and POS integration with comprehensive inventory management.

### Key Features
- **Unified Platform**: Website builder and ecommerce in one platform
- **POS Integration**: Physical and online sales integration
- **Inventory Management**: Real-time inventory tracking across channels
- **Design Focus**: Template-based design system with commerce integration
- **Commerce API**: REST API for ecommerce operations
- **Tax Integration**: Built-in tax calculation with third-party integrations
- **Member Areas**: Customer account and membership functionality

## Core Services

- `SquarespaceOrderSyncService`: Order synchronization and management
- `SquarespaceProductSyncService`: Product catalog operations
- `SquarespaceShopSyncService`: Shop configuration and settings
- `SquarespaceServiceClientFactory`: Authentication and API client management
- `SquarespaceWebhookService`: Webhook event processing

## Composers & Decomposers

**Composers**: `SquarespaceOrderComposer`, `SquarespaceProductComposer`
**Decomposers**: `SquarespaceProductDecomposer` (instance-based), `SquarespaceOrderDecomposer`

## Key Patterns

### Currency Object Pattern
Squarespace requires platform-specific currency amount objects for all pricing fields.

### Contextual Pricing
Support for currency conversion with exchange rates for multi-currency scenarios.

### Fulfillment Mapping
Detailed status transitions: Pending → Confirmed → Fulfilled with payment status tracking.

### Refund Tracking
Separate refund amount tracking with currency object handling.

### Design Integration
Products must integrate with Squarespace's template-based design system.

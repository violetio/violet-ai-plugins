# Rithum Platform Overview

## Platform Summary

Rithum (formerly CommerceHub) is a B2B marketplace platform that connects retailers with suppliers. The platform specializes in drop-shipping and marketplace fulfillment with complex product catalog and order management systems.

### Key Features
- **Drop-shipping Focus**: Supplier-to-consumer fulfillment model
- **Complex Product Types**: Variation parents, children, and single products
- **Multi-tier Status System**: Checkout, payment, and shipping statuses
- **Comprehensive Order Management**: Detailed order lifecycle tracking
- **Supplier Network**: Integration with multiple suppliers and retailers
- **Inventory Management**: Real-time stock synchronization

## Core Services

- `RithumOrderSyncService`: Order synchronization with fallback shipping methods
- `RithumProductSyncService`: Product catalog and variant management
- `RithumShopSyncService`: Shop configuration and supplier setup
- `RithumServiceClientFactory`: Authentication and API client management
- `RithumWebhookSyncService`: Event notification processing

## Composers & Decomposers

**Composers**: `RithumOrderComposer` (modern static architecture ✓)
**Decomposers**: `RithumProductDecomposer`, `RithumOrderDecomposer` (advanced patterns)

## Key Patterns

### Modern Static Architecture
Rithum exemplifies modern patterns with static methods and utility helpers.

### Three-Tier Status System
- **Checkout Status**: Order placement stage
- **Payment Status**: Payment processing stage
- **Shipping Status**: Fulfillment stage

### Product Type Handling
- **VARIATION_PARENT**: Products with child variants
- **STANDARD**: Single products without variants
- **DIGITAL**: Digital/downloadable products
- **BUNDLE**: Product bundles and kits

### Fallback Shipping Methods
Configurable fallback shipping for when primary methods unavailable.

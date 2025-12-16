# Ecommerce Integration Patterns

## Overview

Ecom-integrations coordinates work across two code repositories:
- **EcomSyncService** — Java service with decomposers, composers, webhooks
- **PlatformClientLibrary** — Java library with API clients, models

## The Two Directions

```
┌─────────────────┐                      ┌─────────────────┐
│                 │    DECOMPOSE         │                 │
│    External     │  ────────────────>   │     Violet      │
│    Platform     │    (Platform→Violet) │     Models      │
│                 │                      │                 │
│   (Shopify,     │    COMPOSE           │   (Offer,       │
│    WooCommerce, │  <────────────────   │    Order,       │
│    BigCommerce) │    (Violet→Platform) │    Customer)    │
│                 │                      │                 │
└─────────────────┘                      └─────────────────┘
```

### Decompose (Platform → Violet)
Transform external platform data into Violet's canonical models.

**Examples:**
- Shopify Product → Violet Offer
- BigCommerce Order → Violet Order
- WooCommerce Customer → Violet Customer

### Compose (Violet → Platform)
Transform Violet's canonical models into platform-specific formats.

**Examples:**
- Violet Order → Shopify Order
- Violet Fulfillment → BigCommerce Shipment
- Violet Refund → WooCommerce Refund

## Core Principles

### Static Architecture
All new code must use modern static architecture:
- Static methods, no instance variables
- Pure functions
- No hidden state

### Model-First Workflow
Always create models before sync code:
1. Generate models in PlatformClientLibrary
2. Install models: `mvn clean install -pl PlatformClientModels`
3. Write decomposers/composers in EcomSyncService
4. Test integration end-to-end

### Zero-Ambiguity Principle
**No code is written until every question has an answer.**

The Design → Development transition requires:
- 100% field mappings documented
- 100% status mappings defined
- 100% edge cases identified
- 100% error scenarios planned

## Integration Types

| Type | Description | Discovery Approach |
|------|-------------|-------------------|
| **Platform** | Known systems (Shopify, WooCommerce, BigCommerce) | API documentation analysis |
| **Custom** | Unknown/proprietary systems | Full intake process with partner |
| **Hybrid** | Platform with custom extensions | Platform baseline + custom delta |

## Adding a Platform Integration

1. Create models in PlatformClientLibrary
2. Create service client in PlatformClientLibrary
3. Create decomposers/composers in EcomSyncService
4. Add webhook handlers in EcomSyncService
5. Write comprehensive tests
6. Update platform status tracking

## File Locations

### EcomSyncService
```
platforms/[platform]/
├── *SyncService.java      # Orchestration
├── *Decomposer.java       # Platform → Violet
├── *Composer.java         # Violet → Platform
├── *WebhookController.java # Event handlers
└── CLAUDE.md              # Platform-specific context
```

### PlatformClientLibrary
```
[Platform]ServiceClient/
├── src/main/java/
│   └── io/drizzl/platform/client/
│       ├── [Platform]ServiceClient.java
│       └── models/
└── pom.xml
```

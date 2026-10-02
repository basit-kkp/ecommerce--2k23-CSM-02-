# Sprint 2 — Catalog Data Foundation

## 1. Sprint Goal and Scope Boundary

### Sprint Goal

The goal of Sprint 2 is to build the catalog data foundation for the General Retail E-Commerce Website.

Sprint 2 extends the Sprint 1 database design by adding proper support for categories, products, variants, and SKUs. The catalog foundation will provide the database structure required for future storefront, cart, and checkout functionality.

### In Scope

- Category management
- Product creation and editing
- Product status management
- Product and category relationships
- Product variants
- SKU records
- Unique SKU codes
- SKU price and stock quantity
- Basic authenticated administration
- Database constraints and foreign keys
- Seed data
- Automated validation and authorization tests

### Out of Scope

The following features are planned for Sprint 3 or later:

- Dynamic product specifications
- Asset/image upload
- Public catalog search
- Publication workflows
- Payment gateway integration
- Order placement improvements
- Shipping integration
- Complete shopper checkout flow

---

## 2. Link to Sprint 1 Decisions

Sprint 2 continues the architecture and technology decisions made in Sprint 1.

### Existing Technology Stack

- Frontend: React.js
- Backend: Node.js with Express.js
- Database: PostgreSQL

### Sprint 1 Entities Reused

The Sprint 1 database contains:

- Users
- Categories
- Products
- Orders
- Order_Items
- Cart
- Cart_Items

Sprint 2 extends the catalog part of this design by adding:

- Product Variants
- SKUs

The existing Cart, Cart_Items, Orders, and Order_Items entities remain part of the overall system and will consume the catalog data in later development.

---

## 3. Updated ERD and Data Dictionary

### 3.1 Updated Mermaid ERD

```mermaid
erDiagram

    USERS ||--o{ ORDERS : places
    USERS ||--o| CART : owns

    CATEGORIES ||--o{ CATEGORIES : parent_of
    CATEGORIES ||--o{ PRODUCTS : contains

    PRODUCTS ||--o{ VARIANTS : has
    VARIANTS ||--o{ SKUS : materializes

    PRODUCTS ||--o{ ASSETS : displays

    PRODUCTS ||--o{ CART_ITEMS : selected_as
    SKUS ||--o{ ORDER_ITEMS : sold_as

    ORDERS ||--|{ ORDER_ITEMS : contains
    CART ||--o{ CART_ITEMS : contains

    USERS {
        INTEGER id PK
        VARCHAR email
        VARCHAR password_hash
        VARCHAR first_name
        VARCHAR last_name
        TIMESTAMP created_at
    }

    CATEGORIES {
        INTEGER id PK
        INTEGER parent_id FK
        VARCHAR name
        VARCHAR slug UK
        BOOLEAN active
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    PRODUCTS {
        INTEGER id PK
        INTEGER category_id FK
        VARCHAR name
        VARCHAR slug UK
        TEXT description
        VARCHAR status
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    VARIANTS {
        INTEGER id PK
        INTEGER product_id FK
        VARCHAR option_values
    }

    SKUS {
        INTEGER id PK
        INTEGER variant_id FK
        VARCHAR sku_code UK
        DECIMAL price
        INTEGER stock_quantity
        BOOLEAN active
    }

    ASSETS {
        INTEGER id PK
        INTEGER product_id FK
        VARCHAR storage_key
        VARCHAR role
        VARCHAR alt_text
        INTEGER sort_order
    }

    ORDERS {
        INTEGER id PK
        INTEGER user_id FK
        DECIMAL total_amount
        VARCHAR status
        TIMESTAMP created_at
    }

    ORDER_ITEMS {
        INTEGER id PK
        INTEGER order_id FK
        INTEGER product_id FK
        INTEGER quantity
        DECIMAL unit_price
    }

    CART {
        INTEGER id PK
        INTEGER user_id FK
        TIMESTAMP created_at
        TIMESTAMP updated_at
    }

    CART_ITEMS {
        INTEGER id PK
        INTEGER cart_id FK
        INTEGER product_id FK
        INTEGER quantity
    }
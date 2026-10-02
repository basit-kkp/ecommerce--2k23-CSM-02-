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

3.2 Main Relationships

One category can contain many products.

A category can have an optional parent category.

One product can have zero or more variants.

One variant can have zero or more SKUs.

Each SKU has a unique SKU code.

Each SKU has its own price and stock quantity.

Products can be connected to cart items.

SKUs will be connected to order items for future order processing.

Products can have related assets.


3.3 Data Dictionary

Category

Field	Type	Description

id	INTEGER	Primary key
parent_id	INTEGER	Optional parent category
name	VARCHAR	Category name
slug	VARCHAR	Unique category slug
active	BOOLEAN	Category active status
created_at	TIMESTAMP	Creation time
updated_at	TIMESTAMP	Last update time


Product

Field	Type	Description

id	INTEGER	Primary key
category_id	INTEGER	Foreign key to category
name	VARCHAR	Product name
slug	VARCHAR	Unique product slug
description	TEXT	Product description
status	VARCHAR	Product status
created_at	TIMESTAMP	Creation time
updated_at	TIMESTAMP	Last update time


Variant

Field	Type	Description

id	INTEGER	Primary key
product_id	INTEGER	Foreign key to product
option_values	VARCHAR	Variant option values


SKU

Field	Type	Description

id	INTEGER	Primary key
variant_id	INTEGER	Foreign key to variant
sku_code	VARCHAR	Unique SKU code
price	DECIMAL	SKU price
stock_quantity	INTEGER	Available stock
active	BOOLEAN	SKU availability


3.4 Data Integrity Rules

Category slugs must be unique.

Product slugs must be unique within the catalog.

SKU codes must be unique.

Foreign keys must protect relationships between entities.

Price must use a decimal or integer minor-unit representation.

Floating-point values must not be used for money.

Stock quantity must not become negative through normal administrative updates.

A category cannot become its own ancestor.

Only valid variant combinations should be represented by SKUs.



---

4. Administration Route Table

The administration API will provide protected operations for catalog management.

Method	Route	Purpose

POST	/api/v1/admin/products	Create a product
PATCH	/api/v1/admin/products/:id	Update product information or status
POST	/api/v1/admin/products/:id/skus	Add a validated SKU
PATCH	/api/v1/admin/skus/:id	Update SKU price, stock, or active status
GET	/api/v1/admin/products	List administrative product records
POST	/api/v1/admin/categories	Create a category
GET	/api/v1/admin/categories	Return the category tree


Authentication

Administrative create, update, and delete operations require an authenticated and authorized administrator.

Unauthenticated or unauthorized requests must be rejected.

Example Product Request

{
  "name": "Example Smartphone",
  "slug": "example-smartphone",
  "description": "Example retail product",
  "status": "draft",
  "category_id": 1
}

Example SKU Request

{
  "sku_code": "PHONE-BLK-128",
  "price": 99999.00,
  "stock_quantity": 10,
  "active": true
}

Validation

Duplicate product slugs and duplicate SKU codes must return a clear client error instead of a server traceback.


---

5. Data Integrity and Authorization Decisions

Category Integrity

Categories use stable identifiers and unique slugs.

A category may have an optional parent category. The system must prevent a category from becoming its own ancestor.

Product Integrity

Every product has a unique slug and a category relationship.

A product can have a draft or other defined status according to the catalog workflow.

SKU Integrity

Every SKU has a unique code.

Each SKU stores its own:

Price

Stock quantity

Active status


Negative stock quantities are not allowed through normal administrative updates.

Authorization

Administrative create, update, and delete operations are restricted to authorized administrators.

Regular users must not be allowed to perform administrative catalog write operations.


---

6. Seed Data and Demonstration Instructions

The Sprint 2 seed data will demonstrate the catalog model using representative retail products.

Categories

At least two categories will be included with a parent-child relationship.

Example:

Electronics

Mobile Phones



Products

At least three products will be included.

Example:

1. Example Smartphone


2. Example Laptop


3. Example Headphones



Variants and SKUs

At least one product will contain multiple variants.

At least four valid SKUs will be included.

Example:

Product	Variant	SKU	Stock

Example Smartphone	Black / 128GB	PHONE-BLK-128	10
Example Smartphone	Blue / 128GB	PHONE-BLU-128	5
Example Smartphone	Black / 256GB	PHONE-BLK-256	7
Example Laptop	8GB / 256GB	LAP-8-256	4


An unavailable product combination will not be created as a fake SKU.

Demonstration Flow

The administration demonstration should show:

1. Administrator creates a category.


2. Administrator creates a product.


3. Administrator creates a variant.


4. Administrator creates a SKU.


5. Administrator retrieves the records through the administration API.



Authentication tokens and private URLs must not be included in the documentation.


---

7. Test Strategy, Command, and Result

Automated tests will verify both successful operations and rejection cases.

Required Test Cases

1. Product creation with required fields.


2. SKU creation with required fields.


3. Duplicate product slug rejection.


4. Duplicate SKU code rejection.


5. Category hierarchy validation.


6. Category cycle prevention.


7. Variant and SKU combination validation.


8. Negative stock rejection.


9. Administrative authorization failure for unauthorized requests.



Test Command

The final implementation will document the exact command used to run the automated test suite.

Example:

npm test

Test Result

The final repository will record the test result after the Sprint 2 implementation and verification are completed.


---

8. Known Limitations and Sprint 3 Backlog

Known Limitations

The following functionality is outside the Sprint 2 scope:

Dynamic product specifications

Asset upload

Public catalog search

Publication workflows

Payment gateway integration

Shipping integration

Complete checkout flow


Sprint 3 Backlog

Sprint 3 can build on the Sprint 2 catalog foundation by adding:

Dynamic specifications

Product and variant assets

Public catalog reads

Publication rules

Catalog-to-cart readiness

Further storefront functionality


Sprint 3 should consume the existing catalog tables and SKU identities instead of duplicating product or pricing logic. 
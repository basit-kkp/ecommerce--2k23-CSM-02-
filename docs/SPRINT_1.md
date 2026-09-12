# ecommerce--2k23-CSM-02-
E-Commerce SDLC Assignment — Sprint 1

Architecture & Scope Definition

Project: General Retail E-Commerce Website
Sprint: Sprint 1 — Planning

---

1. Target Audience & Market Focus

1.1 Primary Persona

The primary users of the platform are retail customers who want to purchase different products online.

A typical customer can browse products, search for products, add products to a shopping cart, and place an order through the website.

1.2 Core Pain Point

Customers may find it inconvenient to visit physical stores to search for and purchase different products.

The platform provides a convenient online system where customers can browse products, search for items, manage their shopping cart, and place orders.

1.3 Domain Scope

The project focuses on the General Retail E-Commerce domain.

The platform will allow customers to browse and purchase different types of consumer products through an online shopping website.

---

2. Minimum Viable Product (MVP) Feature Scope

Category| Feature Name| Description| Priority
Authentication| User Registration & Authentication| Users can create an account and securely log in to the website. Password hashing and JWT-based authentication can be used.| High (MVP)
Catalog| Product List & Search| Customers can browse products and search or filter products by category.| High (MVP)
Cart| Cart Management| Customers can add products to the cart, change quantities, and remove products.| High (MVP)
Checkout| Order Processing| Customers can confirm their cart and place an order through the checkout process. A mock payment process can be used.| High (MVP)
Admin| Inventory Control| An administrator can add, update, delete, and manage product inventory.| Medium

---

3. Tech Stack Selection & Justification

3.1 Frontend Framework: React.js

Justification:
React.js will be used to create the user interface of the e-commerce website. It supports reusable components and is suitable for creating product pages, shopping cart pages, login pages, and checkout pages.

3.2 Backend Infrastructure: Node.js with Express.js

Justification:
Node.js with Express.js will be used to develop the backend and APIs. It provides a simple and lightweight approach for handling users, products, carts, and orders.

3.3 Database Management System: PostgreSQL

Justification:
PostgreSQL will be used because the e-commerce system contains structured relationships between users, products, categories, orders, and carts. A relational database is suitable for maintaining primary keys, foreign keys, relationships, and data integrity.

3.4 Caching & Asynchronous Processing: Optional

Redis is not required for the initial MVP. It may be considered in a later stage if caching or background processing becomes necessary.

---

4. Entity-Relationship Diagram (ERD)

The relational database contains the following main entities:

- Users
- Products
- Categories
- Orders
- Order_Items
- Cart
- Cart_Items

4.1 Mermaid ERD

erDiagram

    USERS ||--o{ ORDERS : places
    USERS ||--o| CART : owns

    CATEGORIES ||--o{ PRODUCTS : categorizes

    ORDERS ||--|{ ORDER_ITEMS : contains
    PRODUCTS ||--o{ ORDER_ITEMS : ordered_in

    CART ||--o{ CART_ITEMS : contains
    PRODUCTS ||--o{ CART_ITEMS : added_to

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
        VARCHAR name
        VARCHAR description
    }

    PRODUCTS {
        INTEGER id PK
        INTEGER category_id FK
        VARCHAR name
        VARCHAR description
        DECIMAL price
        INTEGER stock_quantity
        TIMESTAMP created_at
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

4.2 Relationship & Cardinality

- Users → Orders: One user can place many orders. 1:N
- Users → Cart: One user can have one active cart. 1:1
- Categories → Products: One category can contain many products. 1:N
- Orders → Order_Items: One order contains one or more order items. 1:N
- Products → Order_Items: One product can appear in many order items. 1:N
- Cart → Cart_Items: One cart can contain many cart items. 1:N
- Products → Cart_Items: One product can appear in many cart items. 1:N

"ORDER_ITEMS" acts as an associative entity between "ORDERS" and "PRODUCTS", creating an effective N:M relationship.

"CART_ITEMS" acts as an associative entity between "CART" and "PRODUCTS", creating an effective N:M relationship.

---

5. Data Modeling Summary

The database uses primary keys to uniquely identify records and foreign keys to connect related entities.

"ORDER_ITEMS" connects orders with products, while "CART_ITEMS" connects carts with products.

The design uses SQL-compatible data types including "INTEGER", "VARCHAR", "DECIMAL", and "TIMESTAMP".

---

6. Sprint 1 Summary

Sprint 1 defines the initial architecture, functional scope, technology stack, and database structure of the General Retail E-Commerce Website.

The MVP focuses on the essential shopping workflow: user authentication, product browsing and searching, cart management, order processing, and basic inventory management.

The defined ERD provides the database foundation required for implementation in the upcoming sprints.

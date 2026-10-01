Sprint 2 — Architecture & Boundaries
1. Sprint Goal
The goal of Sprint 2 is to define the system architecture and boundaries of the E-Commerce Campus Store.
2. Architecture
The Campus Store will use a modular monolithic architecture.
The system will have different modules such as:
•	User Management
•	Product Management
•	Shopping Cart
•	Order Management
•	Payment
•	Admin Management
These modules will work within one application but will have separate responsibilities.
3. System Boundary
The system boundary defines what is included inside the Campus Store and what is handled by external systems.
Inside the System
•	User registration and login
•	Product browsing
•	Product search
•	Shopping cart
•	Order placement
•	Order management
•	Admin product management
Outside the System
•	Payment gateway
•	Email/SMS services
•	External delivery services
4. Why Modular Monolith?
A modular monolith is suitable for the Campus Store because the project is relatively small and easier to develop and maintain as one application.
It provides clear separation between modules without the additional complexity of microservices.
5. Architecture Comparison
Architecture	Description	Suitability
Monolith	All features are tightly combined in one application	Simple but can become difficult to maintain
Modular Monolith	One application with clearly separated modules	Suitable for Campus Store
Microservices	Application is divided into independent services	More complex for this project
Headless	Frontend and backend are separated through APIs	Useful for flexible frontend development
6. Technology Stack
Layer	Technology
Frontend	HTML, CSS, JavaScript
Backend	Node.js
Database	MongoDB
Version Control	GitHub
7. Module Boundaries
Each module has a specific responsibility:
•	User Module: Handles registration, login, and user information.
•	Product Module: Handles products, categories, prices, and stock.
•	Cart Module: Handles products selected by users.
•	Order Module: Handles order creation and order status.
•	Payment Module: Handles payment-related operations.
•	Admin Module: Allows administrators to manage products and orders.
8. Key Decision
For the Campus Store, the selected architecture is Modular Monolith because it keeps the project simple while maintaining clear boundaries between different system modules.
9. Sprint 2 Outcome
At the end of Sprint 2, the project has a defined architecture, system boundary, module responsibilities, and technology stack.


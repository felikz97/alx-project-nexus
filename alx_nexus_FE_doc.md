# alx-prpject-nexus

# 🛒 E-Commerce Product Catalog Backend (Django + PostgreSQL)

## 📌 Overview

This project implements the backend for an **e-commerce product catalog** system using Django and PostgreSQL. It simulates a real-world scenario focused on scalability, performance, and security. The backend provides REST APIs to manage products, categories, sellers, shopping lists (carts), orders, and user authentication, supporting operations like filtering, sorting, searching, and pagination. API documentation is auto-generated and hosted using Swagger.

## 🎯 Goals

- Build secure and scalable CRUD APIs for managing **e-commerce catalog** entities such as products, categories, orders, shopping lists, and sellers.
- Implement robust user authentication with JWT.
- Enable efficient product discovery using filters, sorting, searching, and pagination.
- Optimize database performance with indexes and query enhancements.
- Deliver comprehensive and developer-friendly API documentation.

## 🛠️ Technologies Used

- **Framework**: Django, Django REST Framework
- **Database**: PostgreSQL
- **Authentication**: JWT (`djangorestframework-simplejwt`)
- **Documentation**: Swagger (via `drf-yasg`)
- **Version Control**: Git (with semantic commits)

## 🔑 Key Features

1. **CRUD Operations for Product Catalog, Sellers, Orders, and Shopping Lists**
   - Products: Add, update, retrieve, delete
   - Categories: Add, update, retrieve, delete
   - Orders: Place, view, manage
   - Shopping List (Cart): Add/remove/update product quantities
   - Sellers: Associate products with seller/store
   - Users: Register, login (JWT-based)

2. **Product Catalog API Enhancements**
   - Filtering by category, price range, and seller
   - Sorting by price, creation date
   - Searching by product name and description
   - Pagination for large product catalogs

3. **Authentication & Security**
   - Token-based authentication (JWT)
   - Token refresh support
   - Secure endpoints with permission handling

4. **API Documentation**

   - Swagger UI and Redoc for API testing and documentation
   - Hosted Swagger available at `/swagger/`

5. **Database Optimization**
   - Indexes on `price`, `category`, and `seller` fields
   - Optimized queries using `select_related()` and `prefetch_related()`

## 🏗️ Overall Design

- Modular Django project structure
- RESTful endpoints using Django REST Framework
- JWT-secured access to protected resources
- Reusable and validated serializers for input/output control
- Pagination, search, and query params for enhanced **catalog API** usability

## 🧮 Database Design (Catalog Focus)

### User

| Field       | Type     | Description             |
|-------------|----------|-------------------------|
| id          | UUID     | Primary Key             |
| username    | string   | Unique username         |
| password    | string   | Hashed password         |

### Category

| Field | Type    | Description      |
|-------|---------|------------------|
| id    | UUID    | Primary Key      |
| name  | string  | Unique category  |
| slug  | string  | URL-friendly key |

### Seller

| Field       | Type     | Description                  |
|-------------|----------|------------------------------|
| id          | UUID     | Primary Key                  |
| name        | string   | Seller or store name         |
| user_id     | foreign key | Linked to User (store owner) |

### Product

| Field       | Type       | Description                     |
|-------------|------------|---------------------------------|
| id          | UUID       | Primary Key                     |
| name        | string     | Product title                   |
| description | text       | Product description             |
| price       | decimal    | Product price                   |
| category_id | foreign key| Linked to Category              |
| seller_id   | foreign key| Linked to Seller                |
| created_at  | timestamp  | Auto-set on creation            |
| updated_at  | timestamp  | Auto-set on update              |

### Order

| Field       | Type         | Description                          |
|-------------|--------------|--------------------------------------|
| id          | UUID         | Primary Key                          |
| user_id     | foreign key  | User who placed the order            |
| product_ids | many-to-many | Products included in the order       |
| total_price | decimal      | Total price of the order             |
| created_at  | timestamp    | Time the order was created           |
| status      | string       | Order status (e.g., pending, shipped)|

### Shopping List (Cart)

| Field       | Type       | Description                          |
|-------------|------------|--------------------------------------|
| id          | UUID       | Primary Key                          |
| user_id     | foreign key| Linked to User                       |
| created_at  | timestamp  | Auto-set on creation                 |
| active      | boolean    | Whether this is the current cart     |

### Shopping List Item

| Field            | Type       | Description                            |
|------------------|------------|----------------------------------------|
| id               | UUID       | Primary Key                            |
| shopping_list_id | foreign key| Linked to ShoppingList                 |
| product_id       | foreign key| Linked to Product                      |
| quantity         | integer    | Quantity of the product in the list    |

## 📡 API Endpoints

### Authentication

| Method | Endpoint             | Description         |
|--------|----------------------|---------------------|
| POST   | `/api/token/`        | Get JWT token       |
| POST   | `/api/token/refresh/`| Refresh token       |
| POST   | `/api/register/`     | User registration   |

### Products (Catalog)

| Method | Endpoint          | Description                             |
|--------|-------------------|-----------------------------------------|
| GET    | `/api/products/`  | List products (filter, search, sort, paginate) |
| POST   | `/api/products/`  | Create product (admin or seller)        |
| GET    | `/api/products/<id>/` | Retrieve product details            |
| PUT    | `/api/products/<id>/` | Update product (admin or seller)    |
| DELETE | `/api/products/<id>/` | Delete product (admin or seller)    |

### Categories

| Method | Endpoint            | Description                        |
|--------|---------------------|------------------------------------|
| GET    | `/api/categories/`  | List all categories                |
| POST   | `/api/categories/`  | Create category (admin)            |
| GET    | `/api/categories/<id>/` | Retrieve category details     |
| PUT    | `/api/categories/<id>/` | Update category (admin)       |
| DELETE | `/api/categories/<id>/` | Delete category (admin)       |

### Orders

| Method | Endpoint           | Description                          |
|--------|--------------------|--------------------------------------|
| GET    | `/api/orders/`     | List user orders                     |
| POST   | `/api/orders/`     | Create a new order                   |
| GET    | `/api/orders/<id>/`| Retrieve specific order details      |
| PUT    | `/api/orders/<id>/`| Update order status (admin)          |
| DELETE | `/api/orders/<id>/`| Cancel/delete order (optional)       |

### Sellers

| Method | Endpoint           | Description                        |
|--------|--------------------|------------------------------------|
| GET    | `/api/sellers/`    | List all sellers                   |
| POST   | `/api/sellers/`    | Register a new seller/store        |
| GET    | `/api/sellers/<id>/`| Retrieve seller details           |
| PUT    | `/api/sellers/<id>/`| Update seller profile             |
| DELETE | `/api/sellers/<id>/`| Delete seller (admin)             |

### Shopping List (Cart)

| Method | Endpoint                           | Description                              |
|--------|------------------------------------|------------------------------------------|
| GET    | `/api/shopping-list/`             | Retrieve current user's active list      |
| POST   | `/api/shopping-list/items/`       | Add item to list                         |
| PUT    | `/api/shopping-list/items/<id>/`  | Update quantity of a list item           |
| DELETE | `/api/shopping-list/items/<id>/`  | Remove item from shopping list           |
| POST   | `/api/shopping-list/checkout/`    | Checkout: convert shopping list to order |

## 🔍 Filtering, Searching, Pagination

### 🔎 Filtering

Products can be filtered using:

- `?category=<category_id>`
- `?min_price=<value>`
- `?max_price=<value>`
- `?seller=<seller_id>`

### 🔤 Searching

Products can be searched via:
- `?search=<keyword>` (matches product name or description)

### ↕️ Sorting

Use Django’s ordering filter:

- `?ordering=price` (asc)
- `?ordering=-price` (desc)
- `?ordering=created_at`

### 📄 Pagination

Uses page number pagination:

- `?page=<number>` (e.g., `/api/products/?page=2`)

---

For deployment, environment variables and `.env.example` should be provided, and API documentation should be publicly hosted and accessible for easy frontend integration and testing.

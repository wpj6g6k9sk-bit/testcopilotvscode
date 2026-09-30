# Mule API Design Overview

This document outlines the architecture and design decisions for the Mule API. The design covers:

1. **Endpoint Architecture** – Resources, paths, and HTTP methods.
2. **Data Model** – Domain entities and JSON contracts.
3. **Authentication & Authorization** – Security mechanisms.
4. **Error Handling & Monitoring** – Resilience and observability.
5. **Deployment & CI/CD** – Build, test, and release pipeline.

## 1. Endpoint Architecture

| Resource | Method | Path | Description |
|----------|--------|------|-------------|
| `/orders` | GET | `/orders` | List all orders (paginated). |
| `/orders` | POST | `/orders` | Create a new order. |
| `/orders/{orderId}` | GET | `/orders/{orderId}` | Retrieve order details. |
| `/orders/{orderId}` | PUT | `/orders/{orderId}` | Update an existing order. |
| `/orders/{orderId}` | DELETE | `/orders/{orderId}` | Delete an order. |
| `/orders/{orderId}/items` | POST | `/orders/{orderId}/items` | Add an item to an order. |
| `/orders/{orderId}/items/{itemId}` | DELETE | `/orders/{orderId}/items/{itemId}` | Remove an item. |
| `/products` | GET | `/products` | List all products. |
| `/products` | POST | `/products` | Create a new product. |
| `/products/{productId}` | GET | `/products/{productId}` | Retrieve product details. |
| `/products/{productId}` | PUT | `/products/{productId}` | Update a product. |
| `/products/{productId}` | DELETE | `/products/{productId}` | Delete a product. |
| `/auth/login` | POST | `/auth/login` | User login (JWT). |

All endpoints follow **REST** principles, use **JSON** payloads, and expose **HATEOAS** links for discoverability.

## 2. Data Model

### Order
```json
{
  "id": "string",
  "customerId": "string",
  "status": "string", // NEW, PROCESSING, SHIPPED, CANCELLED
  "items": [
    { "productId": "string", "quantity": 1, "price": 9.99 }
  ],
  "total": 19.98,
  "createdAt": "ISO8601",
  "updatedAt": "ISO8601"
}
```

### Product
```json
{
  "id": "string",
  "name": "string",
  "description": "string",
  "price": 9.99,
  "stock": 100,
  "createdAt": "ISO8601",
  "updatedAt": "ISO8601"
}
```

### User
```json
{
  "id": "string",
  "username": "string",
  "email": "string",
  "role": "ADMIN|USER",
  "createdAt": "ISO8601",
  "updatedAt": "ISO8601"
}
```

## 3. Authentication & Authorization

- **JWT …
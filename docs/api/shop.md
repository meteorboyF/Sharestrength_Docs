# Shop API

The marketplace for accessibility products and services.

## List Products

Get all available products (public).

```http
GET /api/v1/products
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| category | string | Filter by category |
| min_price | decimal | Minimum price filter |
| max_price | decimal | Maximum price filter |
| page | integer | Page number |

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Ergonomic Keyboard",
            "description": "Accessible keyboard with large keys",
            "price": 89.99,
            "image_url": "/storage/products/keyboard.jpg",
            "in_stock": true,
            "category": "Assistive Technology"
        }
    ]
}
```

## Get Single Product

```http
GET /api/v1/products/{id}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "name": "Ergonomic Keyboard",
        "description": "Accessible keyboard designed for users with limited mobility. Features large keys, high contrast labels, and adjustable tilt.",
        "price": 89.99,
        "image_url": "/storage/products/keyboard.jpg",
        "in_stock": true,
        "stock_quantity": 25,
        "category": "Assistive Technology",
        "specifications": {
            "weight": "1.2 kg",
            "dimensions": "45 x 15 x 3 cm",
            "connectivity": "USB, Bluetooth"
        }
    }
}
```

## Create Order

Place a new order.

```http
POST /api/v1/orders
Authorization: Bearer {token}
Content-Type: application/json

{
    "items": [
        {
            "product_id": 1,
            "quantity": 2
        },
        {
            "product_id": 3,
            "quantity": 1
        }
    ],
    "shipping_address": {
        "street": "123 Main St",
        "city": "Anytown",
        "state": "CA",
        "zip": "12345",
        "country": "USA"
    }
}
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| items | array | Yes | Products to order |
| items[].product_id | integer | Yes | Product ID |
| items[].quantity | integer | Yes | Quantity |
| shipping_address | object | Yes | Delivery address |

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "status": "pending",
        "items": [
            {
                "product_id": 1,
                "name": "Ergonomic Keyboard",
                "quantity": 2,
                "price": 89.99,
                "subtotal": 179.98
            },
            {
                "product_id": 3,
                "name": "Screen Magnifier",
                "quantity": 1,
                "price": 49.99,
                "subtotal": 49.99
            }
        ],
        "total": 229.97,
        "shipping_address": {
            "street": "123 Main St",
            "city": "Anytown",
            "state": "CA",
            "zip": "12345"
        }
    },
    "message": "Order placed successfully"
}
```

## Order Status Values

| Status | Description |
|--------|-------------|
| pending | Order placed, awaiting payment |
| paid | Payment received |
| shipped | Order shipped |
| delivered | Order delivered |
| cancelled | Order cancelled |

## Livewire Components

The shop frontend is built with Livewire components:

| Component | Description |
|-----------|-------------|
| Marketplace | Product browsing grid |
| ProductDetails | Single product view |
| Cart | Shopping cart management |
| Checkout | Order placement flow |

### Cart Functionality

The cart is managed client-side with Livewire state:

- Add items to cart
- Update quantities
- Remove items
- Calculate totals
- Proceed to checkout

### Checkout Flow

1. Review cart items
2. Enter shipping address
3. Confirm order
4. Order created (status: pending)
5. Complete payment
6. Order status updates

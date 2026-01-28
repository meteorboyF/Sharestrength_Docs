# Payments API

Track payments between PWD users (payers) and HelpMates (payees).

## List Payments

Get payment history for the authenticated user.

```http
GET /api/v1/payments
Authorization: Bearer {token}
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| status | string | Filter by status (pending, paid, failed) |
| page | integer | Page number |

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "amount": 50.00,
            "status": "paid",
            "task": {
                "id": 1,
                "title": "Grocery Shopping Assistance"
            },
            "payer": {
                "id": 1,
                "name": "John Doe",
                "type": "user"
            },
            "payee": {
                "id": 2,
                "name": "Jane Smith",
                "type": "helper"
            },
            "paid_at": "2024-01-15T14:00:00Z",
            "created_at": "2024-01-15T12:00:00Z"
        }
    ]
}
```

## Create Payment

Record a new payment for a completed task.

```http
POST /api/v1/payments
Authorization: Bearer {token}
Content-Type: application/json

{
    "task_id": 1,
    "amount": 50.00
}
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| task_id | integer | Yes | Associated task ID |
| amount | decimal | Yes | Payment amount |

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "amount": 50.00,
        "status": "pending",
        "task_id": 1
    },
    "message": "Payment created"
}
```

## Get Payment Summary

Get aggregated payment statistics.

```http
GET /api/v1/payments/summary
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": {
        "total_paid": 500.00,
        "total_pending": 75.00,
        "total_received": 0,
        "payment_count": 12,
        "this_month": {
            "paid": 150.00,
            "pending": 25.00
        }
    }
}
```

## Get Payment Insights

Get detailed payment analytics.

```http
GET /api/v1/payments/insights
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": {
        "by_month": [
            {
                "month": "2024-01",
                "total": 250.00,
                "count": 5
            },
            {
                "month": "2023-12",
                "total": 180.00,
                "count": 4
            }
        ],
        "by_category": [
            {
                "category": "Transportation",
                "total": 200.00,
                "count": 4
            },
            {
                "category": "Shopping",
                "total": 150.00,
                "count": 3
            }
        ],
        "average_payment": 45.00,
        "highest_payment": 100.00,
        "lowest_payment": 20.00
    }
}
```

## Payment Status Values

| Status | Description |
|--------|-------------|
| pending | Payment recorded but not completed |
| paid | Payment successfully processed |
| failed | Payment failed |

## Payment Flow

1. **Task Completed** - HelpMate marks task as complete
2. **Payment Created** - PWD user creates payment record (status: pending)
3. **Payment Processed** - Payment is processed externally
4. **Status Updated** - Payment marked as paid or failed

!!! note
    ShareStrength tracks payment records but does not process actual transactions. Integration with a payment processor (Stripe, PayPal, etc.) would be needed for production use.

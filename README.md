# Payment Service

Microservice for handling payments with idempotency in the Hospital Management System.

## Overview

The Payment Service processes payments with idempotency support to prevent double-charging on retries.

## Features

- ✅ Idempotent payments via `Idempotency-Key` header
- ✅ No double-charging on retries
- ✅ Payment tracking
- ✅ API version `/v1`
- ✅ OpenAPI 3.0 documentation

## Quick Start

### Prerequisites

- Python 3.8+
- pip

### Installation

```bash
pip install -r requirements.txt
```

### Running Locally

```bash
python app.py
```

The service will start on `http://localhost:8006`

### Using Docker

```bash
docker build -t payment-service:latest .
docker run -p 8006:8006 payment-service:latest
```

### Using Docker Compose

```bash
docker-compose up
```

## API Documentation

Once the service is running, visit:
- Swagger UI: http://localhost:8006/docs
- ReDoc: http://localhost:8006/redoc

## Endpoints

- `POST /v1/payments` - Create a payment (requires `Idempotency-Key` header)
- `GET /v1/payments` - List payments (with filters)
- `GET /v1/payments/{payment_id}` - Get payment by ID
- `GET /health` - Health check endpoint

## Idempotency

All payment requests must include an `Idempotency-Key` header. If a payment with the same key already exists, the existing payment is returned instead of creating a duplicate.

Example:
```bash
curl -X POST http://localhost:8006/v1/payments \
  -H "Idempotency-Key: unique-key-123" \
  -H "Content-Type: application/json" \
  -d '{"bill_id": 1, "amount": 525.00, "method": "UPI"}'
```

## Environment Variables

- `PORT` - Service port (default: 8006)
- `DATABASE_URL` - Database connection string (default: sqlite:///./payment.db)

## Kubernetes Deployment

```bash
kubectl apply -f k8s/deployment.yaml
```

## Database Schema

**Payments Table:**
- `payment_id` (Integer, Primary Key)
- `bill_id` (Integer, Foreign Key)
- `amount` (Numeric)
- `method` (String: UPI, CARD, CASH, etc.)
- `reference` (String, Unique - stores idempotency key)
- `paid_at` (DateTime)

## Contributing

This is part of a microservices architecture. For integration with other services, see the main Hospital Management System documentation.

## License

Academic use only.


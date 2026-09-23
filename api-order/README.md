# Ramakrishnan — Order Orchestration API Guide

## API Overview

**Application:** `api-order`  
**Port:** `8081`  
**Owner:** Ramakrishnan (`Ramakrishnan2619`)  
**Branch:** `feature/order-api`

The **Order Orchestration API** serves as the central real-time entry point for customer orders, managing request correlation, high-value priority routing, downstream orchestration across team services (Validation on 8082, Processing on 8083), and live web storefront serving.

---

## Core Flow Architecture

```text
POST /api/orders
      │
      ▼
HTTP Listener (Port 8081) with CORS Enabled
      │
      ▼
Generate / Propagate Correlation ID (CORR-ORD<id>-001)
      │
      ▼
Log Inbound Order & Payload Details
      │
      ▼
Choice Router: Evaluate High-Value Threshold (>= ₹50,000)
      ├── [>= 50,000] ──> OrderType: "PRIORITY", Queue: "priority-orders"
      └── [< 50,000]  ──> OrderType: "STANDARD", Queue: "standard-orders"
      │
      ▼
Downstream Pipeline Orchestration Subflow (Validation 8082 -> Processing 8083)
      │
      ▼
DataWeave Transformation (Contract-Compliant JSON Response)
      │
      ▼
Global Error Handler (On Error Continue with Standardized REJECTED Schema)
```

---

## Completed Checklist

- [x] RAML 1.0 Definition (`src/main/resources/api/api-order.raml`)
- [x] HTTP Listener (`0.0.0.0:8081`) with complete CORS preflight (`OPTIONS`) support
- [x] Health check endpoint (`GET /api/health`)
- [x] Correlation ID Generator & Propagator (`CORR-ORD<id>-001`)
- [x] Choice Router with ₹50,000 high-value threshold
- [x] DataWeave 2.0 transformations adhering strictly to frozen team contract
- [x] Resilient error handling with `on-error-continue`
- [x] Integrated Web Storefront Dashboard (`GET /` and `GET /index.html`)
- [x] Standalone Netlify-ready single-page application for instant resume showcase

---

## Endpoints

### 1. Ingest Order
- **Method:** `POST`
- **Path:** `/api/orders`
- **Headers:** `Content-Type: application/json`, `X-Correlation-ID: (optional)`
- **Request Body:**
  ```json
  {
    "orderId": "ORD1001",
    "customerId": "CUS101",
    "product": "MacBook Pro M3 Max",
    "quantity": 1,
    "amount": 249900,
    "priority": true,
    "email": "customer@example.com"
  }
  ```
- **Response (201 Created):**
  ```json
  {
    "orderId": "ORD1001",
    "customerId": "CUS101",
    "product": "MacBook Pro M3 Max",
    "quantity": 1,
    "amount": 249900,
    "orderType": "PRIORITY",
    "priority": true,
    "status": "RECEIVED",
    "message": "High-value priority order received and dispatched to priority queue",
    "correlationId": "CORR-ORD1001-001",
    "timestamp": "2026-09-23T14:30:00Z"
  }
  ```

### 2. Health Check
- **Method:** `GET`
- **Path:** `/api/health`
- **Response (200 OK):**
  ```json
  {
    "status": "UP",
    "service": "api-order",
    "port": 8081,
    "version": "1.0.0",
    "threshold": 50000
  }
  ```

### 3. Storefront UI
- **Method:** `GET`
- **Path:** `/` or `/index.html`
- Serves the cyber-electric interactive storefront with live cart checkout, bulk CSV ingestion, and real-time Mule trace visualization.

---

## Testing with cURL

```bash
curl -X POST http://localhost:8081/api/orders \
  -H "Content-Type: application/json" \
  -d '{"orderId":"ORD1001","customerId":"CUS101","product":"Laptop","quantity":2,"amount":85000,"priority":true,"email":"customer@example.com"}'
```

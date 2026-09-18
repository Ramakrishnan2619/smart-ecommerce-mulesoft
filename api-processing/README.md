# Harshita — Processing API Guide

## Your API

**Application:** `api-processing`  
**Port:** `8083`  
**Branch:** `feature/processing-api`

## Main Goal

Apply the business routing and asynchronous processing logic.

## Core Flow

```text
POST /api/process-order
        ↓
Log
        ↓
Transform Message
        ↓
Choice Router
    /          \
HIGH-VALUE   STANDARD
    \          /
       VM Queue
```

## Priority Rule

The high-value threshold must be agreed by the team.

```text
IF amount >= agreed threshold
    → PRIORITY
ELSE
    → STANDARD
```

Do not invent or silently change the threshold.

## Build Checklist

- [ ] RAML/API contract understanding
- [ ] APIkit/generated flow
- [ ] HTTP Listener
- [ ] HTTP Request practice
- [ ] DataWeave
- [ ] Transform Message
- [ ] Subflow
- [ ] Flow Reference
- [ ] Choice Router
- [ ] Logger
- [ ] VM Queue
- [ ] Database/File/Email connector practice
- [ ] Scheduler practice
- [ ] On Error Propagate
- [ ] On Error Continue
- [ ] MUnit
- [ ] Auto-Discovery understanding
- [ ] Policy understanding
- [ ] Git PR

## Queue Message

Use the team's agreed message:

```json
{
  "orderId": "ORD1001",
  "customerId": "CUS101",
  "amount": 85000,
  "orderType": "PRIORITY",
  "status": "READY_FOR_FULFILLMENT",
  "email": "customer@example.com"
}
```

## Postman

```text
POST http://localhost:8083/api/process-order
Content-Type: application/json
```

## MUnit

Test:
- Priority routing
- Standard routing
- Boundary amount
- Invalid processing input
- Queue failure
- Error handlers

## Important

The queue name and priority threshold are shared contracts. Do not change them independently.

## Git

```bash
git checkout main
git pull upstream main
git checkout -b feature/processing-api
git add .
git commit -m "feat: implement order processing and routing"
git push origin feature/processing-api
```

Create PR to central `main`.

## Done Means

Routing is correct, queue publishing works, logs exist, failures are controlled, MUnit passes and the contract is unchanged.

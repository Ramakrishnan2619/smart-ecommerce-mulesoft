# Ramakrishnan — Order API Guide

## Your API

**Application:** `api-order`  
**Port:** `8081`  
**Branch:** `feature/order-api`

You own the **Order Orchestration API**, but you are expected to learn the full MuleSoft stack.

## Main Goal

Create the real-time entry point for orders and coordinate the downstream workflow.

## Your Core Flow

```text
POST /api/orders
      ↓
APIkit-generated implementation
      ↓
Log request
      ↓
Validate / call Validation API
      ↓
Call Processing API
      ↓
Return agreed response
```

## Build Checklist

- [ ] Create RAML
- [ ] Publish RAML to Exchange
- [ ] Generate APIkit flows
- [ ] HTTP Listener
- [ ] HTTP Request to downstream API
- [ ] Transform Message / DataWeave
- [ ] Subflow
- [ ] Flow Reference
- [ ] Choice Router practice
- [ ] Logger
- [ ] Error handling
- [ ] On Error Propagate
- [ ] On Error Continue practice
- [ ] MUnit
- [ ] Postman testing
- [ ] Understand Auto-Discovery
- [ ] Understand API policy
- [ ] Git PR

## Shared Request

Use the team's frozen order JSON from the main README.

## Your Responsibilities

1. Receive the order.
2. Generate/propagate correlation ID.
3. Log order ID and correlation ID.
4. Call Validation API on `8082`.
5. Handle validation response.
6. Call Processing API on `8083` when valid.
7. Return the agreed response.
8. Test invalid and downstream-failure cases.

## Important

Do not change shared endpoints, JSON fields or ports without Team Lead approval.

## Test With Postman

```text
POST http://localhost:8081/api/orders
Content-Type: application/json
```

Use the common test order from the main README.

## MUnit

At minimum test:
- Valid order
- Invalid order
- Missing field
- Validation API failure
- Processing API failure
- Error handlers

## Git

```bash
git checkout main
git pull upstream main
git checkout -b feature/order-api

git add .
git commit -m "feat: implement order orchestration API"
git push origin feature/order-api
```

Then create the PR to central `main`.

## Done Means

Your API builds, runs, follows the shared contract, passes MUnit, has meaningful logs, has tested error handling and has a PR ready for review.

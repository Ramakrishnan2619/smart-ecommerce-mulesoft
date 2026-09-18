# Sabreen — Validation API Guide

## Your API

**Application:** `api-validation`  
**Port:** `8082`  
**Branch:** `feature/validation-api`

GitHub accounts:
- `Sabreen-Anjum`
- `SabreenAnjum-02`

## Main Goal

Validate incoming orders using reusable MuleSoft logic.

## Core Flow

```text
POST /api/validate-order
        ↓
Log
        ↓
Validation Subflow
        ↓
Flow Reference
        ↓
DataWeave / Transform Message
        ↓
Validation result
```

## Validate

Check the team's agreed required fields:

- `orderId`
- `customerId`
- `product`
- `quantity`
- `amount`
- `priority`
- `email`

Also validate sensible values such as positive quantity and valid business data according to the agreed contract.

## Build Checklist

- [ ] RAML
- [ ] APIkit-generated flow
- [ ] HTTP Listener
- [ ] HTTP Request practice
- [ ] DataWeave
- [ ] Transform Message
- [ ] Validation Subflow
- [ ] Flow Reference
- [ ] Choice Router practice
- [ ] Logger
- [ ] VM/MQ practice
- [ ] Database/File/Email connector learning
- [ ] Scheduler practice
- [ ] On Error Propagate
- [ ] On Error Continue
- [ ] MUnit
- [ ] Postman
- [ ] Auto-Discovery understanding
- [ ] Policy understanding
- [ ] Git PR

## Error Behavior

Invalid API input should use the agreed error contract and demonstrate **On Error Propagate**.

Use **On Error Continue** for a controlled/recoverable scenario where appropriate and document the reason.

## Postman

```text
POST http://localhost:8082/api/validate-order
Content-Type: application/json
```

## MUnit

Test:
- Valid order
- Missing orderId
- Invalid quantity
- Invalid amount
- Invalid email/business data
- Validation error response
- Error handler behavior

## Git

```bash
git checkout main
git pull upstream main
git checkout -b feature/validation-api
git add .
git commit -m "feat: implement order validation API"
git push origin feature/validation-api
```

Create your PR to central `main`.

## Done Means

Validation works independently, follows the shared contract, logs meaningful events, handles errors, passes MUnit and is ready for integration.

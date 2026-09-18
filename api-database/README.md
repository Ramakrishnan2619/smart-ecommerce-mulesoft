# Harini — Persistence / Database API Guide

## Your API

**Application:** `api-database`  
**Port:** `8085`  
**Branch:** `feature/database-api`

## Main Goal

Persist orders into the team's existing Axion/MySQL cloud database connection.

You are **not only the database person**. Learn and practice the full MuleSoft stack as much as practical.

## Core Flow

```text
POST /api/orders
        ↓
Log
        ↓
Transform Message
        ↓
Database Insert
        ↓
Success response
```

## Proposed Table

`orders`

| Column | Type |
|---|---|
| `order_id` | VARCHAR |
| `customer_id` | VARCHAR |
| `product` | VARCHAR |
| `quantity` | INT |
| `amount` | DECIMAL |
| `priority` | BOOLEAN |
| `email` | VARCHAR |
| `status` | VARCHAR |
| `created_at` | DATETIME |

Confirm the final schema with the team before integration.

## DB Failure Requirement

The selected problem requires:

```text
Database operation
      ↓
Failure
      ↓
On Error Continue
      ↓
Write payload to backup local File
      ↓
Log failure
```

This is a critical demo scenario.

## Build Checklist

- [ ] RAML
- [ ] APIkit/generated flow
- [ ] HTTP Listener
- [ ] HTTP Request practice
- [ ] DataWeave
- [ ] Transform Message
- [ ] Subflow
- [ ] Flow Reference
- [ ] Choice Router practice
- [ ] Logger
- [ ] VM/MQ understanding
- [ ] Database Connector
- [ ] File Connector
- [ ] Email connector practice
- [ ] Scheduler practice
- [ ] On Error Propagate
- [ ] On Error Continue
- [ ] MUnit
- [ ] Auto-Discovery understanding
- [ ] Policy understanding
- [ ] Git PR

## Security

Do not put database username/password in GitHub or README.

Use secure/local configuration.

## Postman

```text
POST http://localhost:8085/api/orders
Content-Type: application/json
```

## MUnit

Test:
- Successful insert
- Invalid data
- DB failure using a mock
- Backup-file behavior
- On Error Continue
- Error response
- Logging

## Git

```bash
git checkout main
git pull upstream main
git checkout -b feature/database-api
git add .
git commit -m "feat: implement order persistence API"
git push origin feature/database-api
```

Create PR to central `main`.

## Done Means

Database insertion works, failure backup works, secrets are protected, MUnit passes, logs exist and the schema matches the shared contract.

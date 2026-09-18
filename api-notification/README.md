# Giridharan — Notification / Support API Guide

## Your API

**Application:** `api-notification`  
**Port:** `8084`  
**Branch:** `feature/notification-api`

## Main Goal

Provide the email/notification capability while also practicing the complete MuleSoft stack.

You are **not only the email person**. Learn and implement the other required MuleSoft concepts in your API as practical.

## Core Flow

```text
POST /api/notify-order
        ↓
Log
        ↓
Transform Message
        ↓
Email Connector / SMTP
        ↓
Notification result
```

## Email

Use SMTP.

Credentials must be entered securely at implementation time.

**Never commit the email password.**

## Notification Content

The email should contain useful order information such as:

- Order ID
- Customer ID where appropriate
- Order status
- Order type
- Confirmation message

The final wording should be agreed by the team.

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
- [ ] Database connector practice
- [ ] File connector practice
- [ ] Scheduler practice
- [ ] On Error Propagate
- [ ] On Error Continue
- [ ] MUnit
- [ ] Auto-Discovery understanding
- [ ] API policy understanding
- [ ] Git PR

## Email Failure

Implement a controlled failure path and use the team's agreed error strategy.

Demonstrate **On Error Continue** where appropriate so a notification failure does not create uncontrolled application behavior.

Log the failure without logging credentials.

## Postman

```text
POST http://localhost:8084/api/notify-order
Content-Type: application/json
```

## MUnit

Test:
- Successful email flow
- Invalid input
- Email connector failure using a mock
- Error handling
- Correct email payload/body

## Git

```bash
git checkout main
git pull upstream main
git checkout -b feature/notification-api
git add .
git commit -m "feat: implement order notification API"
git push origin feature/notification-api
```

Create PR to central `main`.

## Done Means

Notification works, credentials are secure, failures are handled, MUnit passes, logs exist and the API follows the shared contract.

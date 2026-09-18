# 🚀 Smart E-Commerce Order Fulfillment System — MuleSoft Mini-Hackathon

## Team

| Member | Primary API | Port | GitHub |
|---|---|---:|---|
| Ramakrishnan | Order Orchestration API | 8081 | `Ramakrishnan2619` |
| Sabreen | Order Validation API | 8082 | `Sabreen-Anjum`, `SabreenAnjum-02` |
| Harshita | Order Processing API | 8083 | `harshita19-cloud` |
| Giridharan | Notification / Support API | 8084 | `iamgiridharan` |
| Harini | Persistence / Database API | 8085 | `harini-buildon` |

**Central repository:** `smart-ecommerce-mulesoft`  
**Owner:** `Ramakrishnan2619`

---

## 1. Project Goal

Build the selected **Smart E-Commerce Order Fulfillment System** as a complete MuleSoft backend integration solution within the 48-hour hackathon.

The official brief requires RAML/Exchange, Development and Stable environments, API Auto-Discovery and a policy, generated flows, Subflows, Flow References, Choice Routers, DataWeave, VM/message queues, Database/File/Email/Queue connectors, Scheduler, both On Error Propagate and On Error Continue, Loggers, MUnit and successful deployment.

For the selected problem, the required business flow is real-time priority orders plus scheduled bulk standard orders from CSV, validation, CSV-to-JSON transformation, high-value/standard routing, asynchronous queue processing, database insertion, confirmation email and DB-failure backup to a local file.

---

## 2. GitHub Workflow

### Central repository
`https://github.com/Ramakrishnan2619/smart-ecommerce-mulesoft`

### Rules
1. Work on a feature branch, never directly on central `main`.
2. Develop inside your assigned API folder.
3. Commit meaningful changes.
4. Push your branch to your fork.
5. Create a PR to central `main`.
6. Ramakrishnan reviews and merges.
7. If changes are requested, fix them and push again.
8. After a merge, sync local `main`.
9. Do not overwrite another member's work.
10. Do not commit secrets.

### Branches
- `feature/order-api`
- `feature/validation-api`
- `feature/processing-api`
- `feature/notification-api`
- `feature/database-api`

---

## 3. Repository Structure

```text
smart-ecommerce-mulesoft/
├── README.md
├── api-order/
│   └── README.md
├── api-validation/
│   └── README.md
├── api-processing/
│   └── README.md
├── api-notification/
│   └── README.md
├── api-database/
│   └── README.md
├── docs/
├── postman/
└── .gitignore
```

---

## 4. Frozen Team Contracts

Do not change these without Team Lead approval and README update:

- API/application names
- Ports
- Base paths/endpoints
- HTTP methods
- Request/response/error formats
- Required fields and types
- DB table/columns/types
- Queue/VM name and message format
- CSV columns/order
- File locations/names
- Status values
- Priority/high-value threshold
- Correlation ID format

### Fixed local ports
- Order: `8081`
- Validation: `8082`
- Processing: `8083`
- Notification: `8084`
- Database: `8085`

---

## 5. Common Order Contract

### Request
```json
{
  "orderId": "ORD1001",
  "customerId": "CUS101",
  "product": "Laptop",
  "quantity": 2,
  "amount": 85000,
  "priority": true,
  "email": "customer@example.com"
}
```

### Proposed success response
```json
{
  "orderId": "ORD1001",
  "status": "RECEIVED",
  "message": "Order received successfully"
}
```

### Proposed error response
```json
{
  "orderId": "ORD1001",
  "status": "REJECTED",
  "errorCode": "VALIDATION_ERROR",
  "message": "Invalid order"
}
```

These response/error structures are team design choices and should be confirmed before implementation.

---

## 6. Business Rule

The hackathon requires separation of high-value vs standard orders.

**Team must confirm the threshold before implementation:**

`High-value threshold = ₹ __________`

Rule:

```text
IF amount >= agreed threshold
    → HIGH-VALUE / PRIORITY
ELSE
    → STANDARD
```

---

## 7. Queue Contract

Primary messaging approach: **VM Queue**.

RabbitMQ may be considered only if the team decides it adds real value and does not destabilize the required solution.

### Proposed queue message
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

**Queue name:** `________________`

---

## 8. Axion / MySQL Database Contract

The team already has its cloud database/MySQL connectivity.

No database credentials belong in GitHub or this README.

### Proposed table: `orders`

| Column | Type | Required | Key |
|---|---|---|---|
| `order_id` | VARCHAR | Yes | Primary Key |
| `customer_id` | VARCHAR | Yes | |
| `product` | VARCHAR | Yes | |
| `quantity` | INT | Yes | |
| `amount` | DECIMAL | Yes | |
| `priority` | BOOLEAN | Yes | |
| `email` | VARCHAR | Yes | |
| `status` | VARCHAR | Yes | |
| `created_at` | DATETIME | Yes | |

These are the team's proposed common columns; confirm them before freezing the schema.

---

## 9. Bulk CSV

Proposed filename: `bulk_orders.csv`

```csv
orderId,customerId,product,quantity,amount,priority,email
ORD2001,CUS201,Keyboard,2,3000,false,user1@example.com
ORD2002,CUS202,Laptop,1,65000,true,user2@example.com
```

Proposed folders:
```text
input/
processed/
failed/
backup/
```

The Scheduler reads the input file, DataWeave converts CSV to JSON, and the order enters the validation/routing workflow.

---

## 10. Correlation ID

Recommended format:

`CORR-ORD1001-001`

Carry the correlation ID through API calls and logs.

---

## 11. Architecture

```text
Real-Time Order
      |
      v
Order API :8081
      |
      v
Validation API :8082
      |
      v
Processing API :8083
      |
      v
Choice Router
   /        Priority   Standard
   \        /
      v
   VM Queue
      |
      v
Queue Listener
   /          DB       Notification
 :8085       :8084
  |            |
Axion DB      Email

Bulk Path:
Scheduler → File → CSV → DataWeave → Validation → Processing → Queue → DB/Email
```

Do not copy flows between projects. APIs communicate through agreed contracts.

---

## 12. Everyone Learns the Full Stack

All five members should practice, as much as practical:

RAML, APIkit/generated flows, HTTP Listener/Request, DataWeave, Transform Message, Subflows, Flow References, Choice Router, Logger, VM/MQ, Database, File, Email, Scheduler, On Error Propagate, On Error Continue, MUnit, API Auto-Discovery, API policy, Git/PR and integration.

Do not add meaningless components just to tick a checklist.

---

## 13. Error Handling

### On Error Propagate
Use when the operation should fail and the error must be returned to the caller, such as an invalid API request.

### On Error Continue
Use for controlled/recoverable behavior. The selected problem specifically requires DB failure to save the payload to a backup local File without stopping the flow.

Every member must understand and test both.

---

## 14. Logging

Log meaningful milestones:

```text
REQUEST_RECEIVED
VALIDATION_RESULT
TRANSFORMATION_COMPLETED
ROUTING_DECISION
QUEUE_PUBLISHED
QUEUE_CONSUMED
DATABASE_OPERATION
EMAIL_OPERATION
BACKUP_FILE_OPERATION
ERROR
FINAL_RESULT
```

Never log passwords, tokens, API keys or database credentials.

---

## 15. MUnit

Each API should test:

- Success
- Invalid request
- Missing required field
- Business-rule failure
- Downstream failure
- Connector failure
- Error handling
- On Error Propagate
- On Error Continue

---

## 16. API Auto-Discovery and Policy

API Auto-Discovery connects the deployed Mule application to API Manager so the API can be managed and governed.

The team will configure it during the appropriate API management stage.

**API instance ID:** `TO BE CREATED / FILLED LATER`

A policy is a governance/security rule applied through API management. The team should use **Rate Limiting** as the initial policy unless the team agrees otherwise.

The exact API instance ID and deployment-specific values are not hard-coded now because they depend on the team's Anypoint Platform environment.

---

## 17. Development and Stable Environments

The official brief requires Development and Stable environments.

Do not make the team deploy everything at the beginning.

Recommended sequence:

```text
Development
→ build/test
→ integration
→ stabilize
→ Stable/final environment
→ final deployment/demo
```

Environment names/URLs will be filled when the team's Anypoint Platform setup is confirmed.

---

## 18. Secrets

Never commit:

- Email passwords
- SMTP passwords
- Database passwords
- API keys
- Tokens
- Client secrets
- Private keys

Use local/secure properties or environment configuration.

---

## 19. 48-Hour Workflow

### Phase 1 — Setup
Repository, branches, contracts and project folders.

### Phase 2 — Build
Each member develops their API and learns the full stack.

### Phase 3 — Test
Individual API testing with Postman and MUnit.

### Phase 4 — PR
Push branches, create PRs, Team Lead reviews/merges.

### Phase 5 — Integration
Connect the five APIs and test end-to-end.

### Phase 6 — Stabilization
Fix bugs, verify contracts, logging, errors and MUnit.

### Phase 7 — Final Packaging
Only now package/build the JAR.

### Phase 8 — Final Deployment & Demo
Deploy, smoke-test and demonstrate.

**Build first. Test first. Integrate first. Deploy last.**

---

## 20. Final Demo

Show:

- Problem statement
- Architecture
- GitHub branches/PRs
- RAML/Exchange
- APIkit/generated flows
- Auto-Discovery
- Policy
- Real-time order
- Validation
- DataWeave
- Choice Router
- VM queue
- Queue listener
- Axion database
- Email
- DB failure → backup file
- Scheduler
- Bulk CSV
- Logger tracing
- MUnit
- Final JAR
- Final deployment
- End-to-end result

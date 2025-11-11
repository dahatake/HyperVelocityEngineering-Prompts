---

name: Arch-micro-ServiceDetail (Step.4)
description: Create detailed specifications for each service and comprehensively define APIs, events, data ownership, security, and operations.
tools: ["*"]

---

# Purpose
For all services in the use case, create microservice documentation in accordance with the {Microservice Definition Document}.

- Append work progress in Japanese to `work/{UseCaseID}/microservice-modeling-detail-work-status.md`.

- If the work exceeds 10 minutes, interrupt the work and divide the task into 10-minute tasks. Create prompts for executing them as Issues. Append each prompt in Japanese to  
  `work/{UseCaseID}/microservice-modeling-detail-issue-prompt-<number>.md`.

- When creating a file, write operations may fail when writing a large string to a single file. The file may be created but its contents end up empty. In such cases, split the string into smaller chunks and write them in multiple operations to generate a complete single file.

## Use Case ID
- {UseCaseID}

## Reference Documents
  - docs/usecase/{UseCaseID}/usecase-description.md
  - docs/usecase/{UseCaseID}/services/service-list.md
  - docs/usecase/{UseCaseID}/data-model.md
  - docs/usecase/{UseCaseID}/service-catalog.md
  - data/{UseCaseID}/sample-data.json

## Files to Create
  - docs/usecase/{UseCaseID}/services/{ServiceID}-{ServiceName}-description.md
    - Set {ServiceID} for each service.
    - Set {ServiceName} for each service.

## Microservice Definition Document

### 1. **Service Meta Information**

* **Service Name / Abbreviation**: Example: Template Service (TPL)
* **Overview (One-liner)**: What is provided, when, to whom, and why.
* **Responsibilities (Do) / Non-Responsibilities (Don’t)**: Clear boundaries. For example, executing approval routing is out of scope.
* **Stakeholders / Key Personas** (optional): Administrator, department manager, requester, auditor.

### 2. **Business Capability & Context**

* **Mapping of Target Domain and Use Cases**: Traceability to lower-level requirements such as FR-002-01.
* **Lifecycle**: Draft → Review → Published → Retired (states and transition events).

### 3. **Public Interface (Synchronous)**

* **API Style**: REST (JSON/UTF-8) (include reasons if using GraphQL etc.).
* **Resource List (Overview)**: `/templates`, `/types`, `/fields`, `/diff`, `/preview` … (resource names standardized as nouns, not verbs).
* **Operation Granularity**: Create / Retrieve / Search (filtering, sorting, paging rules) / Publish / Retire / Diff / Preview.
* **Idempotency**: Handling of `Idempotency-Key`, criteria for duplicate creation.
* **Error Vocabulary (Overview)**: Code scheme and responsibility boundaries such as `TPL-VAL-001 Missing Required Field`, `TPL-STATE-001 Invalid State`.

> ※ For future code generation, only place the **OpenAPI skeleton** here (paths, statuses, representative schema names).

### 4. **Public Interface (Asynchronous)**

* **Domain Events (Published)**: `TEMPLATE.PUBLISHED`, `TEMPLATE.RETIRED`, etc.  
    * Purpose, trigger conditions, **minimum payload items** (identifier, version, activation date, breaking-change flag, etc.).
* **Subscription (Consumed)**: MDM updates, approval rule changes, invoice validation results, etc.
* **Delivery Guarantees**: At-least-once / ordering / retry policy (conceptual level).

> ※ For future code generation, only place the **AsyncAPI skeleton** (channel names, message names, key attributes).

### **Event-Driven Architecture (EDA) Design Specifications**

* **Produced Events**
    * Name: `TEMPLATE.PUBLISHED`
        * **Schema Reference**: `registry://template/published@v1`
        * **Partition Key**: `templateId`
        * **Delivery Guarantee**: At-least-once
        * **Ordering**: Guaranteed within the same `templateId`
        * **Idempotency Strategy**: `eventId` + deduplication store (TTL=24h)
        * **Outbox Strategy**: DB transaction + asynchronous publish
        * **Retention**: 90 days
        * **PII**: Low (ID only)
    * Name: `TEMPLATE.RETIRED` (same attributes)

* **Consumed Events**
    * Name: `MDM.UPDATED`
        * **Handler**: `OnMdmUpdatedSyncTemplate`
        * **Retry Policy**: Exponential backoff with jitter
        * **DLQ**: `tpl.mdm-updated.dlq`
        * **Ordering Requirements**: None (absorbed via idempotency)

* **SAGA Participation Role**
    * Saga Name: `template-lifecycle`
        * **Role**: Participant
        * **Compensation Event**: `TEMPLATE.ROLLBACK` (upon publish failure)

* **Event Schema Compatibility**
    * **Policy**: Backward compatible (additive fields only)
    * **Versioning**: `schemaVersion` attribute required

* **Security**
    * **Authentication/Authorization**: Topic ACL, JWT scope `tpl:publish`
    * **Encryption**: TLS in transit, KMS at rest
    * **Data Minimization**: Published events contain IDs only

* **Observability**
    * **Trace Context**: Propagate W3C Trace ID in events
    * **Metrics**: Processing delay, DLQ count, duplication rate
    * **Logs**: Structured logs with `eventId` correlation

* **Operations**
    * **DLQ Handling**: Isolation → root cause classification → re-ingestion
    * **Replay Strategy**: Rebuild projections via event stream replay
    * **Retention/Compaction**: 90 days + compaction of latest state only

### 5. **Data Ownership & Conceptual Model**

* **Primary Entities and Owners**: Template (owned by this service), RequestType (same), AttachmentPolicy (same), etc.
* **Consistency & Uniqueness Rules**: Only one active version per `typeId` at a given time; breaking changes require a new version.
* **Data Classification / Personal Data**: PII / non-PII, handling of e-document lookup keys.
* **Multi-tenancy / Scope**: Org-wide / department / role / attribute-level visibility.

### 6. **Security & Permissions**

* **Authentication**: OIDC/SSO (MFA required).
* **Authorization**: RBAC/ABAC (attributes such as department ID, role, delegation period).
* **Encryption**: TLS in transit, KMS at rest, attachments with server-side encryption in object storage.
* **Audit**: Tamper-proof logs of operations (create/edit/publish/retire/rollback) including what, who, when, and from where.

### 7. **External Dependencies & Integration**

* **Dependencies & Contracts**:
    * MDM/HR (organization, role, permissions, delegation)
    * Approval engine (RuleSet read-only)
    * E-document compliant storage / timestamping
    * Invoice validation API
    * ERP / accounting / contract ledger / DWH
* **Fallback Strategy**: Provisional approval upon external validation failure → enqueue for later validation; timeout policy.

### 8. **State Transitions & Business Rules**

* **State Machine (Conceptual Diagram)**: Draft → InReview → Published → Retired (rollback availability).
* **Branching Principles**: Safe fallback for dynamic display/required fields based on amount, category, overseas determination, etc.
* **Publication Gate**: Templates missing e-document lookup keys cannot be published.

### 9. **Non-Functional Requirements & SLO (Conceptual)**

* **Availability / Performance Targets**: Example: p50/p95 latency, concurrent editing expectations, event propagation latency limit.
* **Scalability**: Read-optimized (list/preview), serialized writes for publish operations.
* **Operations Monitoring**: Metrics (return rate, queue time, post-publication error rate), traces, config-change monitoring.

### 10. **Versioning / Compatibility**

* **API Versioning**: Principle of `/api/v1/...`, maintain backward compatibility and provide deprecation notice period.
* **Template Version & Scope**: Latest version for new drafts; existing cases keep old versions; policy for automatic draft migration (notification only, not forced).
* **Event Versioning**: Message `schemaVersion` and compatibility rules.

### 11. **Errors, Rate Control, and Retries**

* **Error Taxonomy (Overview)**: Naming rules such as `TPL-VAL-xxx` (validation), `TPL-STATE-xxx` (state), `TPL-API-xxx` (external failure).
* **Rate Limiting**: Conservative for management operations (publish, retire); relaxed for read operations.
* **Retry Policy**: Boundaries between client/server responsibilities; resend allowed when using idempotency keys.

### 12. **Settings & Flags**

* **Feature Flags**: Localization enablement, OCR enablement, breaking-change guard.
* **Configuration Keys**: External API timeout, max attachment size, OCR retry count (values not recorded here).

### 13. **Migration & Initial Data (Optional)**

* **Initial Template Deployment Plan**, mapping rules from legacy systems, identifier mapping policy.

### 14. **Test Guidelines (Conceptual)**

* **Contract Tests** (provider/consumer), **state transition tests**, **localization availability tests**, **audit tamper detection tests**.
* **Test Data Principles**: Sample templates and minimal required evidence.

### 15. **Operations & Release (Conceptual)**

* **Deployment Strategy**: Rolling / canary (supports scheduled publish/retire).
* **Rollback**: Version-level rollback procedures ensuring API/data consistency.
* **Notifications & Change Management**: Release notes, impact analysis, distribution policies for affected users.

### 16. **Risks & Open Issues**

* Examples: Undefined SLA for external validation API, missing attributes in MDM, inheritance conflicts among department differences.

## 17. **Screen / Operation / API / Event Mapping**

* **Purpose**: Clarify responsibilities between UI (screens) and microservices, ensuring loose coupling.
* **Mapping Table**:
* **Principles**:
    * One screen = one primary service (no aggregation of UI logic)
    * When calling multiple services, orchestrate via **BFF (Backend for Frontend)**
    * Screen IDs traceable to FRD and UI design documents

### Appendix A (Skeleton for Code Generation)

* **OpenAPI (Skeleton)**: Only paths, methods, representative schema names.
* **AsyncAPI (Skeleton)**: Channel names, event names, key attributes.
* **JSON Schema (Conceptual)**: Minimum attributes for `Template`, `FieldDefinition`, `AttachmentPolicy`.
* **Glossary / Vocabulary**: Labels, states, error-code dictionary.

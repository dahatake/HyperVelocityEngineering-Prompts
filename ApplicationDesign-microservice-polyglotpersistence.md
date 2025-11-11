# Application Design - Microservice + Polyglot Persistence

We will create a document that can serve as input information for software engineers to start application development.
From multiple candidate use cases, select one at a time and proceed with application design.

## Tools

GitHub Copilot produces relatively good output regarding Software Engineering.
However, the GitHub Copilot Agent has firewall settings enabled by default and cannot reference external information unless it’s via MCP.
In that regard, Microsoft 365 Copilot’s GPT-5 might also be a good option.

GitHub Copilot has multiple agents. Please choose the one you prefer.

[https://docs.github.com/en/copilot/get-started/features](https://docs.github.com/en/copilot/get-started/features)

* GitHub Copilot Agent Mode
  Copilot embedded in IDEs such as Visual Studio X. It has the advantage of allowing you to select from multiple models.

* GitHub Copilot Coding Agent
  In the case of the Coding Agent, it sometimes behaves like “Deep Research,” which may take a bit of time, but it also has the following advantages:

  * Can be tracked as a GitHub Issue
  * After issuing the Issue, it’s easier to work on other tasks on your local PC, etc.
  * It is easier to create multiple files in a batch-like manner

> [!NOTE]
> We recommend saving the created results as documents in a GitHub repository.

# Step. 1. Create a Microservice Application Definition

If you have information on use cases, you can model various aspects such as screens, services, and data.
From here, we will follow the general approach of microservice design to a certain extent and put the specific related information into text.

## Step. 1.1. Perform Domain Analysis

Here, **select only one** of the created use cases and conduct business domain analysis.

```text
# Role
You are a software architect with advanced expertise and hands-on experience in microservice architecture.
You are expected to make technical decisions logically and practically, with the following perspectives and skills:

- **System-wide structural design**: Based on Domain-Driven Design (DDD) and Clean Architecture principles, perform separation of service responsibilities and optimization of dependencies.
- **Ensuring scalability and availability**: Leverage cloud-native designs (e.g., Kubernetes, service mesh) to ensure system scalability and fault tolerance.
- **Data management strategy**: Properly design inter-microservice data consistency, distributed transactions, event-driven architecture (EDA), etc.
- **Security and operability considerations**: Perform design with operations in mind, including authentication/authorization (OAuth2, JWT), observability, and CI/CD pipelines.
- **Technology selection and trade-off judgment**: In selecting the tech stack, consider performance, maintainability, team skill set, etc., and make rational decisions.

Your role is to provide optimal architectural designs and technical proposals for the given objectives, taking these perspectives into account.

# Objective
- Deeply and carefully analyze the document for {Use Case}, perform domain modeling of business functions, and list out the business domains.

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/architecture-domain-modeling-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Use Case
  - docs/usecase/{Use Case ID}/usecase-description.md

## Output File
  - docs/usecase/{Use Case ID}/domain-analytics.md

# Task
## Domain Modeling
  - Analyze the use case and, **from the DDD perspective, identify the Bounded Contexts**.
  - Clarify the business domains to which each use case belongs and find the natural lines of functional separation.

### Domain Modeling Items

#### 1. Summary

- **Use Case ID**: `{Use Case ID}`
- **Use Case Name**: *e.g., Order Registration and Payment*
- **Scope**: *e.g., From order receipt to payment confirmation*
- **Key Stakeholders**: *e.g., EC customers, sales, accounting*
- **Non-functional highlights (only SLO/SLA category upfront)**: *e.g., p95 latency within 2 seconds, 99.9% availability, at-least-once delivery*

#### 2. Domain ID (Use Case ID)

- Unique Identifier: `{Use Case ID}`

#### 3. Ubiquitous Language

- Define the vocabulary used commonly by domain experts and developers. Avoid term collisions and clarify **meaning differences per context**.

| Term                          | Definition                                             | Example/Notes                            |
| ---------------------------- | ------------------------------------------------------ | ---------------------------------------- |
| Example: Order               | An expression of customer intent to purchase; an entity with a lifecycle | Status: Draft → Placed → Paid            |
| Example: Payment Authorization | The act of obtaining authorization via a payment method | Payment methods: credit card, points     |

#### 4. Entity

- Objects with identifiers. **Persisted and with state transitions**.

*   Example: `Order(id, customerId, orderLines[], status, totalAmount, createdAt, …)`
*   Example: `Payment(id, orderId, method, amount, status, authorizedAt, …)`

#### 5. Value Object

- Equality is determined by **value equality**. Treat as **immutable**.

*   Example: `Money(amount, currency)`
*   Example: `Address(country, postalCode, prefecture, city, line1, line2)`

#### 6. Aggregate and Aggregate Root

- **Consistency boundary** and **transaction boundary**. Enclose invariants here.

*   Example: `Order` aggregate (Root: `Order`): contains `OrderLine`, `totalAmount` equals the sum of lines
*   Example: `Payment` aggregate (Root: `Payment`): `status ∈ {Initiated, Authorized, Captured, Failed}`

#### 7. Domain Service

- **Important business rules** that do not belong to a single entity/value object.

*   Example: `PricingService` (discount/tax logic)
*   Example: `RiskAssessmentService` (credit/fraud detection)

#### 8. Repository

- Persistence/reconstruction of aggregates.

*   Example: `OrderRepository`, `PaymentRepository`

#### 9. Factory

- Responsibility for complex creation/initialization.

*   Example: `OrderFactory.createFromCart(cartId, pricingPolicy)`

#### 10. Bounded Context

- The **boundary** within which the model functions effectively. **Meanings of words and rules are stable**.

*   Example: `Sales` (order entry/quotations)
*   Example: `Billing` (invoicing/payments)
*   Example: `Inventory` (stock)
*   Example: `Customer` (customer)
*   Example: `Fulfillment` (shipping/delivery)

#### 11. Context Map

- Show inter-context relationships (upstream/downstream) and **integration styles (Pub/Sub, API, ACL, Anti-Corruption Layer)**.

*   Example: `Sales` → (**published event**) → `Inventory`, `Billing`, `Fulfillment`
*   Example: `Billing` → (published event) → `Sales`, `Accounting`
*   Example: `Inventory` → (published event) → `Sales`, `Fulfillment`
*   Example: Relationship types: Upstream/Downstream, Published Language, ACL, Open Host Service*

#### 12. Domain Event

- Past-tense events meaningful in the domain. **Tied to aggregate state transitions**.

*   Example: `OrderPlaced`, `OrderCancelled`, `PaymentAuthorized`, `PaymentCaptured`, `StockReserved`, `ShipmentCreated`

#### 13. Notes
 - Special notes, etc.
```

## Step. 1.2. Extract the List of Microservices

Extract the listed domains and entities as microservice candidates.
If you are using GitHub Copilot’s Coding Agent, you may add comments to the Pull Request in step 2.1.
If you are using GitHub Copilot Agent Mode, add to the same chat.

```text
# Role
You are a software architect with advanced expertise and hands-on experience in microservice architecture.
You are expected to make technical decisions logically and practically, with the following perspectives and skills:

- **System-wide structural design**: Based on Domain-Driven Design (DDD) and Clean Architecture principles, perform separation of service responsibilities and optimization of dependencies.
- **Ensuring scalability and availability**: Leverage cloud-native designs (e.g., Kubernetes, service mesh) to ensure system scalability and fault tolerance.
- **Data management strategy**: Properly design inter-microservice data consistency, distributed transactions, event-driven architecture (EDA), etc.
- **Security and operability considerations**: Perform design with operations in mind, including authentication/authorization (OAuth2, JWT), observability, and CI/CD pipelines.
- **Technology selection and trade-off judgment**: In selecting the tech stack, consider performance, maintainability, team skill set, etc., and make rational decisions.

Your role is to provide optimal architectural designs and technical proposals for the given objectives, taking these perspectives into account.

# Objective
- Deeply and carefully analyze all business domains in this {Reference Document} and list the candidates for services to implement as microservices. Follow the {Service Candidate List} format.
- Split each use case or group of use cases into service candidates based on the **Single Responsibility Principle (SRP)**.
- Service candidates should ideally be independently deployable and loosely coupled with other services.
- Diagram all microservices in a context map using Mermaid notation.

- Append the work progress in English to `work/{Use Case ID}/service-design-work-status.md`.

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/architecture-microservice-modeling-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Reference Document
  - docs/usecase/{Use Case ID}/usecase-description.md
  - docs/usecase/{Use Case ID}/domain-analytics.md

## Output File
  - docs/usecase/{Use Case ID}/services/service-list.md

## Service Candidate List

### {Service Candidate ID}: {Service Candidate Name}

**Status**: Candidate/Approved/Designing/Implementing/On Hold/Considering Integration/Rejected/To be Deprecated  
**BC**: {Bounded Context} / **Subdomain**: Core/Supporting/Generic  
**Covered UCs**: {UC-ID, UC-ID,...}  
**Primary Responsibility (SRP)**:  
- {Responsibility 1}
- {Responsibility 2}

**Explicit Non-responsibilities**: {e.g., Does not handle payment processing}

#### Domain Model
- **Aggregates/Entities**: {e.g., Order, OrderLine}
- **Published Events**: {e.g., OrderPlaced, OrderCancelled}
- **Subscribed Events**: {e.g., PaymentCaptured}
- **Invariants/Constraints**: {e.g., Total amount ≥ 0}

#### Data Ownership / Storage
- **Owned Data Boundaries**: {bulleted}
- **Data Classification/Sensitivity**: {Public/Internal/Sensitive/Confidential, APPI compliance?}
- **Read/Write Characteristics**: {e.g., R:W=4:1, append-only time series}
- **Storage Choice/Reason**: {e.g., Postgres (transactions)}
- **Retention/Archiving**: {e.g., 7 years, delete PII after 36 months}
- **RPO/RTO**: {e.g., 5 min / 30 min}

#### Interface / Contract
- **Synchronous APIs**: {main endpoints}
- **Asynchronous Contracts**: {topics/schemas}
- **Contract Versioning/Compatibility Policy**: {backward/forward compatible}
- **Idempotency/Consistency**: {strategy}

#### Inter-service Interaction
- **Downstream Dependencies**: {e.g., PaymentSvc, InventorySvc}
- **Upstream Dependencies**: {e.g., ShippingSvc}
- **Communication Style (Reason)**: {sync/async/mixed + reason}
- **Context Map Relationship**: {e.g., Conformist}

#### Non-functional
- **SLO/SLI**: {e.g., 99.95%, p95 200ms}
- **Scaling Characteristics**: {e.g., 2k rps, 5x during year-end}
- **Fault Tolerance**: {retries/timeouts/circuit breakers}
- **Observability**: {metrics/logs/traces}

#### Security/Compliance
- **AuthN/AuthZ**: {e.g., OIDC + RBAC}
- **Data Protection**: {encryption at-rest/in-transit}
- **Audit/Regulations**: {APPI/PCI, etc.}

#### Implementation/Deployment/Operations
- **Tech Stack**: {language/runtime}
- **Deployment Unit**: {K8s/Serverless, etc.}
- **Region/DR**: {e.g., APNE1 primary, APNE2 DR}
- **Release Strategy**: {Blue/Green/Canary}
- **Runbook**: {URL}

#### Decision-making
- **Business Value**: {qualitative/quantitative}
- **Risks/Uncertainties**: {enumerate}
- **Sizing Estimate**: {person-weeks/lead time}
- **Priority Score (formula/value)**: {e.g., Value×2 + Urgency + Dependency resolution×1.5 = 8.5}
- **Alternatives / Split-Merge**: {description}

#### Decision Log & Open Items
- **Important Decisions**: {ADR links}
- **Hypotheses/Assumptions**: {list}
- **Unresolved Items (Owner/Due Date)**: {list}
- **Next Actions**: {list}
```

Up to this point it’s just listed out. If you’d like to diagram, try Mermaid, etc. The following is merely an “example.” In microservices, you can also ask Copilot which diagrams are effective.

# Step. 2. Create the Data Model

Creating the data model is very useful. By extracting from the business requirements flow up to this point, it becomes easier for users to imagine usage as a prototype and to take ownership. Also, if connecting to existing data sources is difficult, you can proceed with application development without being dragged down by it.

```text
# Role
You are a world-class software architect. You are proficient in designing diverse data models and distributed systems, and can build a data foundation that flexibly and robustly scales as needed—from prototypes for a few people to production systems serving hundreds of millions.

## Your Skill Set
- You are proficient in designing **various types of data models (relational, document, graph, time series, etc.)**.
- You can design **flexible and scalable databases that can handle small scale (a few users) to global scale (hundreds of millions of users)**.
- You are required to design architectures considering **performance, availability, scalability, and maintainability**.
- Accurately understand user requirements and constraints (e.g., real-time, cost, cloud environment), and perform optimal technology selection and design based on them.

# Objective
- Deeply and carefully analyze all entities used in this use case and list them. Refer to the documents in {Reference Document}.
- Diagram the data model for all entities in Mermaid notation.

- Design so that each service has its own datastore and clarify data ownership.
- Assume eventual consistency, and consider patterns such as event sourcing and CQRS.
- Create English sample data for all entities used in this use case in JSON format—10 records per entity—and save them to {Sample Data File} as separate data per entity.

- Append the work progress in English to `work/{Use Case ID}/data-model-design-work-status.md`.

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/data-modeling-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Reference Document
  - docs/usecase/{Use Case ID}/usecase-description.md
  - docs/usecase/{Use Case ID}/domain-analytics.md
  - docs/usecase/{Use Case ID}/services/service-list.md

## Output File
  - docs/usecase/{Use Case ID}/data-model.md

## Sample Data File
  - data/{Use Case ID}/sample-data.json
```

# Step. 3. Create the Screen List and Transition Diagram

We now move into screen design.

```text
# Role
You are a world-class software engineer and an exceptional UX/UI designer. You are well-versed in user-centered design (UCD) and modern web technologies (React, TypeScript, Tailwind CSS, Next.js, etc.), and you design and implement web applications that reconcile business value and user experience. You pursue accessibility, responsive design, performance optimization, and aesthetically refined UIs.

## Behavioral Guidelines:
- Deeply understand user goals and context, and propose optimal UX/UI
- Write code that is readable, highly reusable, and adheres to the latest best practices
- Ensure the design is intuitive and visually refined
- Always consider accessibility (WCAG) and internationalization (i18n)
- Reflect feedback quickly and continuously improve

# Objective
- Deeply and carefully analyze and create a list of screens and a screen transition diagram that users will use for this use case. Follow the format in {Output Format}.
- Diagram the screen transition diagram in Mermaid notation.
- Ensure that the portal screen can transition to all use cases. Switch tabs at the top of the screen.
- **Never display {Use Case ID} as-is on the screen. Display the use case name instead.**

- Append the work progress in English to `work/{Use Case ID}/screen-design-work-status.md`.

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/ui-modeling-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Reference Document
  - docs/usecase/{Use Case ID}/usecase-description.md
  - docs/usecase/{Use Case ID}/services/service-list.md
  - docs/usecase/{Use Case ID}/data-model.md

## Output File
  - docs/usecase/{Use Case ID}/screen/screen-list.md

## Output Format

### 1. Screen List
List screens in the following format and include the following information:
- screen_id: unique ID per screen
- screen_name: screen name
- description: brief description of the screen
- function_type: main function (e.g., input form, approval screen, completion screen, etc.)

### 2. Screen Transition Diagram
Diagram the screen transition diagram in Mermaid notation.

### 3. Error Handling
If the identification or transition of screens is ambiguous or cannot be determined from the materials, clearly state this as a note or caution.
```

# Step.4. Create the Service Catalog Table

This is the document we ultimately wanted to create!
Assign IDs for identification to screens, services, and data. Going forward, link the documents that will be created as individual specifications.

Do not forget that an outline of **functions and APIs** has been created in the work steps.

This mapping table alone may be a good candidate for a “review & improve” prompt.

[Review & Improve Prompt](/Advanced Techniques/README.md#4-let-the-prompt-improve-itself)

```text
# Objective
Highly analyze all documents in {Reference Document}, and create, for the application built with a microservices architecture for this use case, a catalog mapping:
Screen → **In-screen Function → In-screen Processing or API Call → Data Managed by the API**
Provide it as a concrete and clear tabular format.
Be sure to adhere to the principles of microservice architecture in your design.

## Requirements:

- For each screen, organize the following:
  - Screen ID
  - Screen Name
  - Major functions within the screen (e.g., search, register, delete, etc.)
  - Explicitly state whether the function is “in-screen processing” or an “API call”
  - For API calls, include the API ID, API name, endpoint, HTTP method, and main parameters
  - Clearly state the data (SoT) managed by the API
- Finally, also add the responsibilities of the API service and the managed data

Additionally, if possible, include:

- A high-level mapping table that allows an overview of the relationship between screens and APIs
- Best practices and caveats for design (idempotency, conflict control, bulk operations, etc.)

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/service-catalog-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Reference Document
  - docs/usecase/{Use Case ID}/usecase-description.md
  - docs/usecase/{Use Case ID}/services/service-list.md
  - docs/usecase/{Use Case ID}/data-model.md
  - docs/usecase/{Use Case ID}/screen/screen-list.md

## Output File
  - docs/usecase/{Use Case ID}/service-catalog.md
```

# Step.5. Create Prompts for Each Component Optimized for Generative AI

> [!NOTE]
> This will work to some extent as-is, but it can probably be further optimized as a prompt…

Based on the use case information created in Step 3, create prompts optimized for generative AI.

## Step.5.1. Create Screen Definition Documents

Screen definition documents are created as structured information that is strongly conscious of existing documents and will likely be easy for “humans” to understand.

```text
# Role
You are a world-class software engineer and an exceptional UX/UI designer. You are well-versed in user-centered design (UCD) and modern web technologies (React, TypeScript, Tailwind CSS, Next.js, etc.), and you design and implement web applications that reconcile business value and user experience. You pursue accessibility, responsive design, performance optimization, and aesthetically refined UIs.

## Behavioral Guidelines:

- Deeply understand user goals and context, and propose optimal UX/UI
- Write code that is readable, highly reusable, and adheres to the latest best practices
- Ensure the design is intuitive and visually refined
- Always consider accessibility (WCAG) and internationalization (i18n)
- Reflect feedback quickly and continuously improve

# Objective
Create screen definition documents for software implementation.

# Task
- Deeply analyze and interpret the screen definition documents for all screens in {Screen List}, and create them while **strictly** complying with {Screen Instruction Sheet} as a checklist. Refer to the documents in {Reference Document}.
- Be sure to append all data from {Sample Data File} as data. Having data included will allow immediate use after creating the screens. Ensure this data can be easily removed later when API integration is implemented.

- Append the work progress in English to `work/{Use Case ID}/screen-doc-implementation-work-status.md`.

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/screen-detail-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Screen List
  - docs/usecase/{Use Case ID}/screen/screen-list.md

## Reference Document
  - docs/usecase/{Use Case ID}/usecase-description.md
  - docs/usecase/{Use Case ID}/services/service-list.md
  - docs/usecase/{Use Case ID}/data-model.md
  - docs/usecase/{Use Case ID}/service-catalog.md

## Output File
  - docs/usecase/{Use Case ID}/screen/{Screen-ID}-{Screen-Name}-description.md
    - Set {Screen-ID} for each screen’s ID
    - Set {Screen-Name} for each screen’s name

## Sample Data File
  - data/{Use Case ID}/sample-data.json

## Screen Instruction Sheet

### 1. Overall Evaluation

* Evaluate the clarity of the prompt’s purpose and goals.
* Show that the acceptance criteria are testable.
* Analyze the staged addition of functionality (foundation → extension → constraints).
* Remove ambiguity and reinforce non-functional requirements, fairness, and UX.

### 2. Specification Rigor

#### Core Functionality

* **Input**: user word list, arbitrary grid size (default 12×12, range 6–20).
* **Placement**: 8 directions (N, NE, E, SE, S, SW, W, NW), both forward and backward. Retry a fixed number of times upon placement failure. Unused cells are random A–Z.
* **Detection**: mouse (click → drag → release), touch (long press → swipe → release). “Found” state for straight-line matches.
* **Completion**: when all words are found, stop the timer and present a “Create New Puzzle” button.

#### Timer & Leaderboard

* **Start**: auto-start immediately after generation.
* **Stop**: stop upon discovery of the last word.
* **Format**: `MM:SS.mmm` (ms precision, display rounded).
* **Recorded items**: name, time, number of words, size.
* **Sorting**: togglable ascending/descending (time/word count/name). In ties, break by **time → word count → name** with a stable sort required.
* **Fairness**: separate rankings per size × word count.

#### Input Constraints & Errors

* **Word length ≤ min(rows, columns)**. On violation, immediately show “Please ensure words are within the size.”
* **Character set**: A–Z (case-insensitive; remove spaces/hyphens).
* **Duplicate words**: either de-duplicate or specify as a warning.
* **Max number of words**: e.g., 5–20; if exceeded or unplaceable, show “Reduce the number of words or increase the size.”

#### Data Model & Algorithm

* **Model**: `Grid`, `Word{text, path}`, `Score{name, timeMs, wordCount, gridSize}`.
* **Placement**: random start + direction; retry on failure (limit = area × 2).
* **Randomness**: varies each time (future extension to allow fixed seed).
* **Performance target**: 12×12 with 10 words → generate within 100ms.

#### UX / A11y

* **Keyboard operation**: arrow movement + Enter to select, Esc to cancel.
* **Color vision support**: combine color with underline/bold.
* **Screen reader**: announce progress with `aria-live`.
* **Focus management**: ensure tab order.

### Security & Privacy

* Sanitize name input, max length 24 characters, control characters prohibited.
* Storage scope is local only. Do not share or transmit.

#### Non-functional Requirements

* Responsiveness: detection response within 16ms.
* Persistence: save up to 100 entries in localStorage.
* Offline: expandable to PWA.

#### Anti-requirements (Out of Scope)

* Hints, multiplayer, server sync, strict cheat detection are excluded.

### 3. Ambiguities and Resolution

* “Diagonal” = the four directions NE / NW / SE / SW.
* “Drag” = a continuous cell-following operation.
* No pause for the timer.
* Tied scores must always result in a single ranking position.

### 4. Example Acceptance Criteria (Gherkin-like)

* **Word length boundary**: on a 10×10 grid, length 10 → success; length 11 → show error.
* **Operation**: match via continuous cell drag → “found” state + checkmark in bank.
* **Timer**: counts up from `00:00.000` immediately after generation; stops when all words are found.
* **Ranking**: ascending by time; in ties, by word count → name order.

### 5. Risks and Trade-offs

* Too many words × small size → propose automatic enlargement.
* Color-only emphasis is NG → compensate with styling.
* Local storage has low cheat resistance → state as acceptable.

### 6. Room for Expansion

* Difficulty presets, category-based word lists, dictionary validation, fixed seed, PWA support, shared ranking API.

### 7. Final Checklist

* Are the goal, completion conditions, and operations clear at a glance?
* Are 8 directions + reverse directions defined?
* Are boundary values and error messages explicitly stated?
* Are timer specifications (start/stop/rounding) rigorous?
* Is the leaderboard fair, with sorting + tie handling defined?
* Are A11y and non-functional requirements covered?
* Is out-of-scope declared?
```

Reference:
[https://docs.github.com/en/copilot/tutorials/easy-apps-with-spark](https://docs.github.com/en/copilot/tutorials/easy-apps-with-spark)

## Step. 5.2. Create Microservice Definition Documents

From the service candidates, select one and create a document compliant with the microservice definition.

Prompt:

```text
# Objective
For all services in the use case, create microservice documents compliant with {Microservice Definition}.

- Append the work progress in English to `work/{Use Case ID}/service-doc-implementation-work-status.md`.

- If the work time exceeds 10 minutes, suspend the work, split this task into tasks per 10 minutes, and create prompts to execute them as Issues. Append each prompt in English to `work/microservice-detail-issue-prompt-<number>.md`.

- When creating files, writing a large string into a single file may fail. The file may be created but its content is empty. In such cases, split the string and perform the write in multiple operations to output to a single file.

## Use Case ID
- UC-xxx

## Reference Document
  - docs/usecase/{Use Case ID}/usecase-description.md
  - docs/usecase/{Use Case ID}/services/service-list.md
  - docs/usecase/{Use Case ID}/data-model.md
  - docs/usecase/{Use Case ID}/service-catalog.md
  - data/{Use Case ID}/sample-data.json

## Output File
  - docs/usecase/{Use Case ID}/services/{Service ID}-{Service Name}-description.md
    - Set {Service ID} for each service’s ID
    - Set {Service Name} for each service’s name

## Microservice Definition

### 1. **Service Meta Information**

*   **Service Name / Short Name**: e.g., Template Service (TPL)
*   **One-liner Overview**: What it provides “when, to whom, and why.”
*   **Responsibilities (Do) / Non-responsibilities (Don’t)**: Clarify boundaries. Executing **approval routing** is a non-responsibility, etc.
*   **Stakeholders / Key Personas** (optional): administrator, department manager, requester, auditor.

### 2. **Business Capability / Context**

*   **Mapping to target domain and use cases**: Traceability to lower-level requirements such as FR-002-01.
*   **Lifecycle**: Draft → Review → Publish → Retire (states and transition events).

### 3. **Public Interfaces (Synchronous)**

*   **API Style**: REST (JSON/UTF-8) (if GraphQL, etc., provide reasoning).
*   **Resource List (Overview)**: `/templates`, `/types`, `/fields`, `/diff`, `/preview` … (**use nouns rather than verbs**).
*   **Operation Granularity**: Create / Retrieve / Search (filter/sort/paging conventions) / Publish / Retire / Diff retrieval / Preview.
*   **Idempotency**: Handling of `Idempotency-Key`, criteria for duplicate creation.
*   **Error Vocabulary (Overview)**: Code taxonomy and responsibility boundaries such as `TPL-VAL-001 Missing required`, `TPL-STATE-001 Invalid state`.

> *For later code generation, put only the **OpenAPI skeleton** here (paths, statuses, representative schema names).*

### 4. **Public Interfaces (Asynchronous)**

*   **Domain Events (Produced)**: e.g., `TEMPLATE.PUBLISHED`, `TEMPLATE.RETIRED`.
    *   Purpose, trigger conditions, **minimum payload items** (identifier, version, effective-from, breaking-change flag, etc.).
*   **Subscriptions (Consumed)**: MDM updates, approval rule changes, invoice verification results, etc.
*   **Delivery Guarantees**: At-least-once / ordering / resend policy (conceptual level).

> *For later code generation, put only the **AsyncAPI skeleton** here (channel names, message names, key attributes).*

### **Event-Driven Architecture (EDA) Design Specification**

*   **Produced Events**
    *   Name: `TEMPLATE.PUBLISHED`
        *   **Schema Ref**: `registry://template/published@v1`
        *   **Partition Key**: `templateId`
        *   **Delivery Guarantee**: At-least-once
        *   **Ordering**: Guaranteed within the same `templateId`
        *   **Idempotency Strategy**: `eventId` + dedup store (TTL = 24h)
        *   **Outbox Strategy**: DB transaction + async publish
        *   **Retention**: 90 days
        *   **PII**: Low (ID only)
    *   Name: `TEMPLATE.RETIRED` (same attributes)

*   **Consumed Events**
    *   Name: `MDM.UPDATED`
        *   **Handler**: `OnMdmUpdatedSyncTemplate`
        *   **Retry Policy**: Exponential backoff + jitter
        *   **DLQ**: `tpl.mdm-updated.dlq`
        *   **Ordering Requirement**: None (absorbed by idempotency)

*   **SAGA Participation Role**
    *   Saga Name: `template-lifecycle`
        *   **Role**: `participant`
        *   **Compensation Event**: `TEMPLATE.ROLLBACK` (on publish failure)

*   **Event Schema Compatibility**
    *   **Policy**: Backward compatible (field additions only)
    *   **Versioning**: `schemaVersion` attribute required

*   **Security**
    *   **AuthN/AuthZ**: Topic ACLs, JWT scope `tpl:publish`
    *   **Encryption**: TLS in transit, KMS at rest
    *   **Data Minimization**: Publish events contain IDs only

*   **Observability**
    *   **Trace Context**: Propagate W3C Trace ID in events
    *   **Metrics**: Processing latency, DLQ count, duplication rate
    *   **Logs**: Structured logs + `eventId` correlation

*   **Operations**
    *   **DLQ Handling**: Isolate → classify cause → re-ingest
    *   **Replay Strategy**: Rebuild projections by replaying event streams
    *   **Retention/Compaction**: 90 days + compress to latest state only

### 5. **Data Ownership / Model (Conceptual)**

*   **Primary Entities and Owners**: Template (owned by this service), RequestType (same), AttachmentPolicy (same), etc.
*   **Consistency/Uniqueness Rules**: Only one active version at the same time for the same `typeId`; breaking changes require a new version, etc.
*   **Data Classification / Personal Information**: PII/non-PII, handling of e-Document Act search keys.
*   **Multitenancy / Scope**: Company-wide / department / role / attributes (visibility granularity).

### 6. **Security / Permissions**

*   **Authentication**: OIDC/SSO (assume MFA).
*   **Authorization**: RBAC/ABAC (attributes: department ID, role, delegation period).
*   **Encryption**: TLS in transit, KMS at rest; attachments use server-side encryption of object storage.
*   **Audit**: **Tamper-proof records** of operations (create/edit/publish/retire/rollback: what/who/when/where).

### 7. **External Dependencies / Integration**

*   **Dependencies and Contracts**:
    *   MDM/HR (org, positions, permissions, delegation)
    *   Approval engine (RuleSet reference only)
    *   e-Document Act-compliant storage / timestamp
    *   Invoice verification API
    *   ERP/accounting/contract ledger/DWH
*   **Fallback Strategy**: If external verification fails, grant provisional approval → **follow-up verification queue**; policy for timeouts.

### 8. **State Transitions / Business Rules**

*   **State Machine (Conceptual)**: Draft → InReview → Published → Retired (Rollback allowed/denied).
*   **Principles for Conditional Branching**: Safe-side fallback for dynamic display/required fields based on amount, category, overseas determination, etc.
*   **Publish Gate**: Templates lacking e-Document Act search keys cannot be published, etc.

### 9. **Non-functional / SLO (Conceptual)**

*   **Availability / Performance Targets**: e.g., p50/p95 latency, assumptions about concurrent editing, upper bound on event propagation delay.
*   **Scalability**: Optimize for reads (lists/previews); serialize writes (publish).
*   **Operations Monitoring**: Metrics (rate of rejections, dwell time, post-publish error rate), tracing, configuration change monitoring.

### 10. **Versioning / Compatibility**

*   **API Versioning**: Principle of `/api/v1/...`, maintain backward compatibility and **deprecation notice period**.
*   **Template Version and Applicability**: New tickets use the latest version; existing cases retain old versions / auto-migrate drafts (notification only, no forced updates).
*   **Event Versioning**: `schemaVersion` in messages and compatibility policy.

### 11. **Errors / Rate Control / Retries**

*   **Error Taxonomy (Overview)**: Naming conventions such as `TPL-VAL-xxx` (validation), `TPL-STATE-xxx` (state), `TPL-API-xxx` (external failure) with **responsibility boundaries**.
*   **Rate Limiting**: Conservative for administrative operations (publish/retire), more lenient for reads.
*   **Retry Policy**: Responsibility demarcation between client/server; allow resends when using **idempotency keys**.

### 12. **Settings / Flags**

*   **Feature Flags**: Localization enablement, OCR enablement, destructive-change guard.
*   **Configuration Keys**: External API timeouts, max attachment size, OCR retry count (**values omitted here**).

### 13. **Migration / Seed Data (Optional)**

*   **Plan for seeding initial templates**, **principles for mapping** from legacy systems, and identifier mapping policy.

### 14. **Testing Guidelines (Conceptual)**

*   **Contract tests** (Provider/Consumer), **state transition tests**, **localization availability tests**, **audit tamper detection tests**.
*   **Test Data Principles**: sample templates and minimal required evidence.

### 15. **Operations / Release (Conceptual)**

*   **Deployment Strategy**: Rolling/Canary (support **time-scheduled** publish/retire).
*   **Rollback**: **Version-level** rollback procedure that maintains API and data consistency (conceptual).
*   **Notifications / Change Management**: Release notes, impact analysis, distribution policy to target users.

### 16. **Risks / Open Issues**

*   Examples: undefined SLA of external verification API, missing attributes in MDM, conflicts in inheritance of departmental differences, etc.

## 17. **Mapping of Screens, Operations, APIs, and Events**

*   **Purpose**: Clarify the responsibilities of the UI (screens) and microservices, and ensure a loosely coupled design.
*   **Mapping Table**:
*   **Principles**:
    *   1 screen = 1 primary service (do not aggregate UI logic)
    *   When calling multiple services, orchestrate via **BFF (Backend for Frontend)**
    *   Screen IDs are traceable with FRD and UI design documents

### Appendix A (Attach “skeletons” for code generation)

*   **OpenAPI (Skeleton)**: paths, methods, representative schema names only.
*   **AsyncAPI (Skeleton)**: channel names, event names, key attributes only.
*   **JSON Schema (Conceptual)**: minimum attributes for `Template`, `FieldDefinition`, `AttachmentPolicy`.
*   **Glossary / Vocabulary**: dictionary of labels, states, and error codes.
```

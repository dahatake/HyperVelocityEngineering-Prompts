# Creating a Web Application

We will create a typical HTML-based web application and deploy it to Azure.
Upload any reference design documents and similar files to a GitHub repository and have the GitHub Copilot Coding Agent work by referring to that URL.
Via the MCP Server, we will check best practices and specifications. We will also read and create resources on Azure.

* Code to create

  * Web UI
  * REST API
  * Data-based

* Resources to create

  * Creation of Azure resources
  * Configure CI/CD from GitHub for the service code

* Architecture design / implementation and review

  * Refer to Microsoft Learn’s MCP Server to perform the latest Azure architecture design.

## Step.1. Data

### Step.1.1. Selecting Azure data stores

Perform appropriate data storage for each entity. Create candidate services that are optimal within Microsoft Azure.

Here we adopt an architecture of **Polyglot Persistence**, which is optimal for cloud usage.

```text
# Role
You are a world-class software architect. You are well-versed in diverse data models and distributed system design, and can build data platforms that flexibly and robustly scale as needed—from prototypes for a handful of users to production systems for hundreds of millions.

## Your role and skill set
- You are proficient in designing **a wide variety of data models (relational, document, graph, time series, etc.)**.
- You can design **flexible, scalable databases** that support **small scale (a few users) to global scale (hundreds of millions)**.
- Architecture design must consider **performance, availability, scalability, and maintainability**.
- Accurately understand user requirements and constraints (e.g., real-time needs, cost, cloud environment) and select and design optimal technologies based on them.

# Objective
- Determine the Microsoft Azure service for storing all entities. Analyze the use cases at a high level, and be sure to reference the {Execution Plan} and {Guidelines} to decide which data of each entity should use which Microsoft Azure service. Also produce a concrete and persuasive explanation of why that Microsoft Azure service was chosen.

- For Microsoft Azure services and architecture, also refer to information obtained from the `MCP Server` below.
  - MicrosoftDocs

- Append work progress in English to `work/{Use Case ID}/data-implementation-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/data-azure-service-selection-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

# Guidelines
- First, **analyze all** {Reference Documents} with high accuracy and create a clear, executable step-by-step execution plan that aligns with the objective. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability. The execution plan must be created in a **separate file** so it can be run in parallel, considering DevOps principles for multi-person application development. Then execute the tasks.
- When multiple design options are possible, present and compare their advantages and disadvantages.
- Adopt `Polyglot Persistence` architecture
  - Polyglot Persistence is an **architecture that uses the optimal data store for different parts of the application**. This optimizes performance, scalability, and development efficiency.

## Use Case ID
- UC-xxx

## Reference Documents
- docs/usecase/{Use Case ID}/usecase-description.md
- docs/usecase/{Use Case ID}/data-model.md
- docs/usecase/{Use Case ID}/services/service-list.md

## File to create
- docs/usecase/{Use Case ID}/AzureServices-data.md

## Execution Plan
Below is a step-by-step explanation of **detailed procedures** when designing Polyglot Persistence.

### 1. Organize requirements and classify data

First, organize overall application requirements and classify the data from the following perspectives:

- **Nature of data**: structured/unstructured, relational/document/graph, etc.
- **Access patterns**: read-heavy/write-heavy/frequent updates
- **Consistency requirements**: strong consistency/eventual consistency
- **Scalability**: whether horizontal scaling is needed
- **Need for transactions**: whether ACID properties are required

### 2. Select data stores

For each classified data type, select the optimal data store. Representative options are:

| Data Type     | Recommended Data Store            | Usage Examples                     |
|---------------|-----------------------------------|------------------------------------|
| Relational    | SQL Database PostgreSQL, MySQL    | User info, transactions            |
| Document      | Azure Cosmos DB                   | Product catalog, blog posts        |
| Key-value     | Redis, Azure Table                | Cache, session management          |
| Graph         | Neo4j, Azure Cosmos DB            | Social networks, recommendation    |
| Time series   | InfluxDB, TimescaleDB             | IoT data, logs                     |
| Search engine | Azure AI Search                   | Full-text search, log analysis     |

### 3. Design data models

For each data store, design the following:

- Schema (as needed)
- Indexes
- Relationships (or references)
- Policies for normalization/denormalization
---

## 4. Strategies for data consistency and synchronization

When using multiple data stores, ensuring consistency is critical. Consider the following strategies:

- **Event sourcing**: record changes as events and reflect them in each store
- **CDC (Change Data Capture)**: detect changes and propagate to other stores
- **Dual write**: the application writes to multiple stores simultaneously (use with caution)

### 5. Documentation and knowledge sharing

- Clearly state the purpose of each data store and the reasons for its design
- Prepare developer guidelines
- Create operational runbooks

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

### Step.1.2. Deploying data services to Azure and registering sample data

Based on the Azure services created, proceed to create Azure services.
Refer to `Microsoft Learn`’s `MCP Server` to create and run `Azure CLI` commands.

```text
# Tasks
- In Microsoft Azure, create the services specified under {Data Stores}. For the latest and detailed specifications of Microsoft Azure services, be sure to search for information in the {MicrosoftDocs} MCP Server and consider it thoroughly.
- First, analyze the {Data Stores} documentation with high accuracy and create a clear, executable execution plan aligned with the objectives. Use structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability.

- The creation of Microsoft Azure services will be done by creating Azure CLI scripts. For the latest and detailed specifications and sample code of Azure CLI, be sure to refer to {MicrosoftDocs} via the MCP Server. Save the created script as a Linux-runnable file in {Location to save Azure CLI scripts}.
- Execute that script using the `Azure MCP Server`.
- Also create a script to install tools and packages required by the Azure CLI script. Save that file to `infra/{Use Case ID}/create-azure-data-resources-prep.sh`.

- After creating the Microsoft Azure services, convert the sample data into appropriate formats for each Microsoft Azure service and create a Linux-runnable bulk data registration script, then execute it. Save the created script to {Location to save data registration scripts}.

- If creation of Microsoft Azure services is successful, append to the {Service Catalog} document a table with service ID, microservice name, service type, service URL, service ID, and service region.

- Append work progress in English to `work/{Use Case ID}/data-azure-deploy-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/data-azuredeployment-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

- Append a feature overview and application startup steps in English to `/README.md`.

## Use Case ID
- UC-xxx

## Data Stores
- docs/usecase/{Use Case ID}/AzureServices-data.md

## Location to save Azure CLI scripts
- infra/{Use Case ID}/create-azure-data-resources.sh

## Location to save data registration scripts
- data/{Use Case ID}/data-registration-script.sh

## Service Catalog
- docs/usecase/{Use Case ID}/service-catalog.md

## Technical Specifications
- Resource group name: `dahatake-{Use Case ID}`
- Region: Japan East
  - If unavailable, select Japan West, East Asia, or South East Asia.
- Always create with the minimum configuration for scale settings.
  - If a service has serverless, choose it.
  - If a service has autoscale, choose it.
- Use {Service Name} for instance names.

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

## Step.2. Creating microservices

Create REST API endpoints.

Create REST API endpoints that reference the created databases. Build them on Azure Functions. Azure Functions are suitable as a runtime for REST APIs in most scenarios.

### Step.2.1. Selecting Azure hosting for services

Select an appropriate hosting environment for each service. Create candidate services that are optimal within Microsoft Azure.

```text
# Role
You are a software architect with advanced expertise and practical experience in microservice architecture. You are well-versed in designing and implementing Microsoft Azure, and you can demonstrate leadership in building scalable, highly available cloud-native applications. You are required to make technical decisions logically and practically with the following perspectives and skills:

- **System-wide structural design**: perform separation of responsibilities and optimization of dependencies for services based on Domain-Driven Design (DDD) and Clean Architecture principles.
- **Ensuring scalability and availability**: leverage cloud-native designs (e.g., Kubernetes, service mesh) to ensure system scalability and fault tolerance.
- **Formulating data management strategies**: appropriately design data consistency between microservices, distributed transactions, and event-driven architecture (EDA).
- **Considering security and operability**: design with operations in mind, including authentication/authorization (OAuth2, JWT), observability, and CI/CD pipelines.
- **Technology selection and trade-off judgment**: in selecting the tech stack, make rational decisions considering performance, maintainability, and the team’s skill set.

# Objective
- Determine the Microsoft Azure service for hosting all services. Analyze the use cases at a high level, and be sure to reference the {Guidelines} to decide which service should use which Microsoft Azure service. Also produce a concrete and persuasive explanation of why that Microsoft Azure service was chosen.

- For Microsoft Azure services and architecture, **be sure** to refer to information obtained from the `MCP Server` below.
  - MicrosoftDocs

- Append work progress in English to `work/{Use Case ID}/services-azure-compute-design-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/data-azure-compute-selection-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

# Guidelines
- First, **analyze all** {Reference Documents} with high accuracy and create a clear, executable step-by-step execution plan that aligns with the objective. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability. The execution plan must be created in a **separate file** so it can be run in parallel, considering DevOps principles for multi-person application development. Then execute the tasks.
- When multiple design options are possible, present and compare their advantages and disadvantages.

## Use Case ID
- UC-xxx

## Reference Documents
- docs/usecase/{Use Case ID}/usecase-description.md
- docs/usecase/{Use Case ID}/data-model.md
- docs/usecase/{Use Case ID}/services/service-list.md

## File to create
- docs/usecase/{Use Case ID}/AzureServices-services.md

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

### Step.2.2. Selecting additional Azure services used by each service

Select additional Azure services that would be useful for each service. For example, for a chatbot, you might use generative AI (Azure OpenAI Service).

```text
# Role
You are a software architect with advanced expertise and practical experience in Microsoft Azure. You are well-versed in designing and implementing Microsoft Azure and can demonstrate leadership in building scalable, highly available cloud-native applications. You are required to make technical decisions logically and practically with the following perspectives and skills:

- **System-wide structural design**: perform separation of responsibilities and optimization of dependencies for services based on Domain-Driven Design (DDD) and Clean Architecture principles.
- **Ensuring scalability and availability**: leverage cloud-native designs (e.g., Kubernetes, service mesh) to ensure system scalability and fault tolerance.
- **Formulating data management strategies**: appropriately design data consistency between microservices, distributed transactions, and event-driven architecture (EDA).
- **Considering security and operability**: design with operations in mind, including authentication/authorization (OAuth2, JWT), observability, and CI/CD pipelines.
- **Technology selection and trade-off judgment**: in selecting the tech stack, make rational decisions considering performance, maintainability, and the team’s skill set.

# Objective
- Analyze and evaluate the use case and {Service Definition Documents} at a high level to determine the additional Microsoft Azure services described under `External dependencies / integrations`. Refer to the {Guidelines} and decide which service’s features should use which Microsoft Azure service. Also produce a concrete and persuasive explanation of why that Microsoft Azure service was chosen.
- For example, for a chatbot, use generative AI (Azure OpenAI Service).
- Exclude Microsoft Azure services specified under {Deployed Services}.

- For Microsoft Azure services and architecture, **be sure** to refer to information obtained from the `MCP Server` below.
  - MicrosoftDocs

- Append work progress in English to `work/{Use Case ID}/services-additional-azureservices-design-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/data-azure-additional-selection-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

# Guidelines
- First, **analyze all** {Reference Documents} with high accuracy and create a clear, executable step-by-step execution plan that aligns with the objective. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability. The execution plan must be created in a **separate file** so it can be run in parallel, considering DevOps principles for multi-person application development. Then execute the tasks.
- When multiple design options are possible, present and compare their advantages and disadvantages.

## Use Case ID
- UC-xxx

## Service Definition Documents
  - docs/usecase/{Use Case ID}/services/{Service ID}-{Service Name}-description.md
    - {Service ID}: set each service’s ID
    - {Service Name}: set each service’s name

## Deployed Services
- docs/usecase/{Use Case ID}/AzureServices-services.md
- docs/usecase/{Use Case ID}/AzureServices-data.md

## Reference Documents
- docs/usecase/{Use Case ID}/usecase-description.md
- docs/usecase/{Use Case ID}/services/service-list.md

## File to create
- docs/usecase/{Use Case ID}/AzureServices-services-additional.md

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

### Step.2.3. Creating additional Azure capabilities

Based on the selection document for the additional Azure capabilities created, proceed to create Azure services.
Refer to `Microsoft Learn`’s `MCP Server` to create and run `Azure CLI` commands.

```text
# Tasks
- In Microsoft Azure, create the services specified under {Additional Services}. For the latest and detailed specifications of Microsoft Azure services, be sure to search for information in the {MicrosoftDocs} MCP Server and consider it thoroughly.
- First, analyze the {Additional Services} documentation with high accuracy and create a clear, executable execution plan aligned with the objectives. Use structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability.

- The creation of Microsoft Azure services will be done by creating Azure CLI scripts. For the latest and detailed specifications and sample code of Azure CLI, be sure to refer to {MicrosoftDocs} via the MCP Server. Save the created script as a Linux-runnable file in {Location to save Azure CLI scripts}.
- Execute that script using the `Azure MCP Server`.
- Also create a script to install tools and packages required by the Azure CLI script. Save that file to `infra/{Use Case ID}/create-azure-additional-resources-prep.sh`.

- If creation of Microsoft Azure services is successful, append to the {Service Catalog} document (in the services section) a table with service ID, service name, Azure service name, feature name, feature type, Azure service URL, and service region.

- Append work progress in English to `work/{Use Case ID}/data-azure-additionalservices-deploy-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/data-azure-additional-deployment-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

- Append a feature overview and application startup steps in English to `/README.md`.

## Use Case ID
- UC-xxx

## Additional Services
- docs/usecase/{Use Case ID}/AzureServices-services-additional.md

## Location to save Azure CLI scripts
- infra/{Use Case ID}/create-azure-additional-resources

## Service Catalog
- docs/usecase/{Use Case ID}/service-catalog.md

## Technical Specifications
- Resource group name: `dahatake-{Use Case ID}`
- Region: Japan East
  - If unavailable, select Japan West, East Asia, or South East Asia. If still unavailable, any region is acceptable.
- Always create with the minimum configuration for scale settings.
  - If a service has serverless, choose it.
  - If a service has autoscale, choose it.
- Use {Service Name} for instance names.
- LLM
  - Azure OpenAI Service
  - Models such as OpenAI’s GPT-5, the latest models

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

### Step.2.4. Creating the code for each service

Create the application code for the service layer as the backend.

```text
# Objective
- Refer to the {Microservice Definition Documents} and create the program code for **all** services in the {Service Creation Folder}.
- First, analyze the {Microservice Definition Documents} with high accuracy and create a clear, executable step-by-step plan aligned with the objectives. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability. Then execute the tasks.
- For the latest and detailed information about the {Technical Specifications}, be sure to obtain the necessary information using the {MicrosoftDocs} MCP Server.
- Based on the {Service Catalog} information, **connect to all Microsoft Azure services** without fail. Always access and obtain all Microsoft Azure resource information using the {Azure} MCP Server to access the {Microsoft Azure resources}. Refer also to the {Azure Services} documents for connection info to each Microsoft Azure service or instance.
- Be sure to create unit test program code. The unit test code must include a simple web page that inputs argument data for the API deployed on Azure and displays the API’s return value. Save unit test code in the {Unit Test Creation Folder}.

- Append work progress in English to `work/{Use Case ID}/api-implementation-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/service-implementation-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

- Append a feature overview and application startup steps in English to `/README.md`.

## Use Case ID
- UC-xxx

# Microservice Definition Documents
- docs/usecase/{Use Case ID}/services/{Service ID}-[Service Name]-description.md

## Reference Documents
- docs/usecase/{Use Case ID}/usecase-description.md
- docs/usecase/{Use Case ID}/services/service-list.md
- docs/usecase/{Use Case ID}/data-model.md

## Service Catalog
- docs/usecase/{Use Case ID}/service-catalog.md

## Azure Services
- docs/usecase/{Use Case ID}/AzureServices-data.md
- docs/usecase/{Use Case ID}/AzureServices-services-additional.md
- docs/usecase/{Use Case ID}/AzureServices-services.md

## Service Creation Folder
- api/{Use Case ID}/{Service ID}-{Service Name}/
  - {Service ID}: set each service’s ID
  - {Service Name}: set each service’s name

## Unit Test Creation Folder
- test/{Use Case ID}/api

## Technical Specifications
- Azure Functions
  - C#
  - Use the latest version supported by Azure Functions
  - SKU: Flex Consumption plan
- LLM
  - Azure OpenAI Service
  - Models such as OpenAI’s GPT-5, the latest models

## Microsoft Azure resources
- Resource group name: `{Resource Group Name}`

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

### Step.2.5. Creating Azure Compute

For the code of each created service, create and deploy each Azure Compute service.

```text
# Objective
- Using the {Executable microservice code} implemented programs, and referring to the {Service Catalog} document, create each Microsoft Azure service and deploy them.
- Do not modify existing code.
- Do not recreate Microsoft Azure services that have already been created.
- Create Azure CLI scripts to create Microsoft Azure services, authenticate them with the `Azure MCP Server`, then execute them. For the latest and detailed specifications of Azure CLI, be sure to refer to {MicrosoftDocs} via the MCP Server. Save the created script as a Linux-runnable file in {Location to save Azure CLI scripts}.
- Also create a script to install tools and packages required by the Azure CLI script. Save that file to `infra/{Use Case ID}/create-azure-api-resources-prep.sh`.

- Implement continuous delivery. For items deployable from GitHub, create GitHub Actions workflows and deploy to Microsoft Azure. For items that cannot implement CI/CD, upload the program code to each created Microsoft Azure service.

- If creation of Azure services is successful, append to the {Service Catalog} document a table with service ID, microservice name, Azure service name, service type, service URL, service ID, and service region.

- Be sure to create unit test program code for **all services running on Azure**. For APIs deployed to Azure Functions, create unit test code as a simple web page that inputs API arguments and displays API return values. Save unit test code in the {Unit Test Creation Folder}.

- Append work progress in English to `work/{Use Case ID}/api-azure-deploy-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/data-azure-compute-deployment-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

- Append a feature overview and application startup steps in English to `/README.md`.

## Use Case ID
- UC-xxx

## Executable microservice code
- api/{Use Case ID}/{Service ID}-{Service Name}/
  - {Service ID}: set each service’s ID
  - {Service Name}: set each service’s name

## Location to save Azure CLI scripts
- infra/{Use Case ID}/create-azure-api-resources.sh

## Service Catalog
- docs/usecase/{Use Case ID}/service-catalog.md

## Unit Test Creation Folder
- test/{Use Case ID}

## Technical Specifications
- Resource group name: `{Resource Group Name}`
- Region: Japan East
  - If unavailable, select Japan West, East Asia, or South East Asia.

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

## Step.3. Creating the UI

When using `GitHub Spark`, write all sample data and documents directly into the prompt.

> [!IMPORTANT]
> GitHub Spark supports **React** and **TypeScript** only.

This will be used as an Issue for the GitHub Copilot Coding Agent.
Assign the Issue to Copilot, and write content like the following in the Issue comments.

> [!IMPORTANT]
> Here, we assume the use of either an SPA or static HTML.

```text
# Role
You are a world-class software engineer and an outstanding UX/UI designer. You are well-versed in user-centered design (UCD) and the latest web technologies (React, TypeScript, Tailwind CSS, Next.js, etc.), and you design and implement web applications that balance business value and user experience. You pursue accessibility, responsive design, performance optimization, and aesthetically refined UI.

## Guidelines for action
- Deeply understand the user’s goals and context and propose optimal UX/UI
- Write code that is readable, reusable, and compliant with the latest best practices
- Ensure the design is intuitive and visually sophisticated
- Always consider accessibility (WCAG) and internationalization (i18n)
- Quickly incorporate feedback and continuously improve

# Objective
- Create the web application screens based on all {Screen Definition Documents}.
- First, analyze the {Screen Definition Documents} with high accuracy and create a clear, executable step-by-step plan aligned with the objectives. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure completeness, clarity, and traceability of the screen definitions. The execution plan must be created in a **separate file** so it can be run in parallel, considering DevOps principles for multi-person application development. Then execute the tasks.
- Work on one screen at a time.
- Be sure to design each screen’s components based on the {Screen Transition Diagram}. Create a categorized collapsible menu to ensure smooth navigation between screens.
- Separate screen structures by persona. For example, display different screens for users and system administrators.

- From all web application screens, implement calls to the REST API endpoints running on Azure Compute services deployed for each service. Refer to the {Service Catalog} and ensure you connect to the endpoints of **all** deployed Azure services.

- For the authentication screen, make it possible to log in regardless of the input. Implementation will be done later.

- Append work progress in English to `work/{Use Case ID}/screen-implementation-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/screen-implementation-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

## Use Case ID
- UC-xxx

## Screen list
- docs/usecase/{Use Case ID}/screen/screen-list.md
  - The {Screen Transition Diagram} is also included

## Screen Definition Documents
- docs/usecase/{Use Case ID}/screen/{Screen ID}-description.md

## Reference Documents
- docs/usecase/{Use Case ID}/usecase-description.md
- docs/usecase/{Use Case ID}/data-model.md
- docs/usecase/{Use Case ID}/services/service-catalog.md
- data/{Use Case ID}/sample-data.json

## Service Catalog
- docs/usecase/{Use Case ID}/service-catalog.md

## Creation Folder
- app/{Use Case ID}

## Technical Specifications
- {UI implementation technology. If not specified, HTML5 and JavaScript only}

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.

## Notes
- Describe a feature overview and application startup steps in English in `/README.md`.
```

Below are examples of multiple patterns. You do not have to use them all.

### (Option) Step.3.0.1. Display multiple web screens at once

> [!NOTE]
> This is merely an example of screen creation. You may skip it.

When displaying multiple web screens at once, create an Issue like the following and have the GitHub Copilot Coding Agent do the work.

```text
Create a [Production Adjustment Application] that can view the following application screens at once.

# Creation Folder
## Creation Folder
- `{app/<folder-name>}`

# Technical Specifications
- Referenced applications
  - [Production Adjustment Agent]
    - Folder name: adjustment-agent
  - [KPI Monitoring Application]
    - Folder name: kpi-monitoring-app
- Use HTML5 only to reference each screen. Do not create original screens.
- Create tabs at the top of the screen listing the application names. Switching tabs displays that application below.
- Specify URLs for referencing each application as absolute paths from the root.
- Make it deployable to Azure Static Web Apps
- Delete all existing GitHub Actions

## Notes
- Append a feature overview and application startup steps in English to `/README.md`.
```

### (Option) Step.3.0.2. (Option) Sample of a multi-agent group chat

> [!NOTE]
> This is merely an example of screen creation. You may skip it.

This is a sample of a multi-agent group chat.

```text
In conjunction with the [KPI Monitoring Application], create a screen where AI agents discuss whether production changes are possible and you can check the progress of that discussion.

# Creation Folder
- app/adjustment-agent

# Technical Specifications
- For demo. Create sample data and use it
- HTML5 only
- The screen is in chat format
- As actors, AI agents perform a group chat: JP Factory Agent, US Factory Agent, Sales General Management Agent, Sales Local Management Agent, Mr. Hatakeyama (human), Mr. Enami (human)
  - Factory agents comment on whether production adjustments are possible based on current production results and production capacity
  - Sales agents comment based on store sales
  - Humans interrupt the conversation between factory and sales agents and make human-like production quantity adjustments
- Business reasons requiring production adjustment
  - In JP, the rainy season is lasting longer than usual; demand for Ice A is expected to be lower than last year, and Snack B is selling
  - Factory A, which was producing Snack B for JP, has no extra capacity; plant substitution production is required
  - At JP retailers of Ice A, continuous line production at Factory D cannot be guaranteed; coordination across other businesses is required
- Add a button on the screen to “Issue a change adjustment instruction.” When the button is pressed, execute the following processing
- Afterwards, create an execution plan. The execution plan will execute dynamically created steps on the screen in order
- When the button is pressed, multiple agents comment on which data they should change
- After adjustment, create a summary of the adjustment results and display it on the screen

## Notes
- Append a feature overview and application startup steps in English to `/README.md`.
```

## Step.3.1. Deploying the Web Application

Deploy the created web application to Azure Static Web Apps.

> [!IMPORTANT]
> Here, we assume the use of either an SPA or static HTML. If HTML screens are generated on the web server side, deployment to Azure Web Apps is recommended.

```text
# Tasks
- Deploy the web application code to Microsoft Azure’s `Azure Static Web Apps`.
- Create an Azure CLI script to create the `Azure Static Web Apps` service and execute that script. For the latest and detailed specifications of Azure CLI, be sure to refer to {MicrosoftDocs} via the MCP Server. Save the created script as a Linux-runnable file in {Location to save Azure CLI scripts}. 
- Also create a script to install tools and packages required by the Azure CLI script. Save that file to `infra/{Use Case ID}/create-azure-webui-resources-prep.sh`.

- Implement continuous delivery. Create a GitHub Actions workflow and deploy to Azure Static Web Apps. For the latest and detailed specifications of GitHub Actions, be sure to refer to {MicrosoftDocs} via the MCP Server.

- If creation of Azure services is successful, append the web application URL to the {Service Catalog} document.

- Append work progress in English to `work/{Use Case ID}/WebUI-azure-deploy-work-status.md`.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/azure-service-deploy-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

- Append a feature overview and application startup steps in English to `/README.md`.

## Use Case ID
- UC-xxx

## Location to save Azure CLI scripts
- infra/{Use Case ID}/create-azure-webui-resources.sh

## Service Catalog
- docs/usecase/{Use Case ID}/service-catalog.md

## Technical Specifications
- Resource group name: {Resource Group Name}
- Region: East Asia
  - If unavailable, select Japan West, East Asia, or South East Asia.
- Create with the minimum configuration for scale settings.
  - If a service has serverless, choose it.
  - If a service has autoscale, choose it.

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding user intent, seek confirmation before major decisions.
- Also include alternatives and future expansion plans to provide material for easy decision-making.
- Always keep the balance of quality, security, cost, and schedule in mind.
```

If creation of Azure Static Web Apps is not possible, try the following steps.

> ### Step 1: Create Azure resources
>
> cd infra/{Use Case ID}
> ./create-azure-webui-resources.sh
>
> ### Step 2: Copy the deployment token (from script output)
>
> ### Step 3: Set in GitHub Secrets
>
> #### 1. Settings > Secrets and variables > Actions
>
> #### 2. New repository secret
>
> #### 3. Name: AZURE_STATIC_WEB_APPS_API_TOKEN
>
> #### 4. Value: Paste the copied token

## Step.4. Architecture Review

Leverage information from Microsoft’s official documentation to review the deployed architecture.

```text
# Objective
- Perform high-level analysis of all components of {Microsoft Azure resources to be reviewed}.
- Perform high-level analysis of which services are deployed to which Microsoft Azure services, and describe them in Mermaid notation and detailed text. Refer also to {Reference Documents}.
- Perform an architecture review based on Microsoft best practices.
- Perform a security review based on Microsoft best practices.
- Be sure to refer to information obtained from the `MicrosoftDocs` MCP Server.
- Save the review results to {Location to save review results}.
- First, create a clear, executable step-by-step plan consistent with the objective. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability. Then execute the tasks.
- When multiple review approaches are possible, present and compare their advantages and disadvantages.

- If the working time exceeds 10 minutes, pause work and split this task into 10-minute tasks, creating prompts to run as Issues. Append each prompt in English to `work/azure-architecture-review-issue-prompt-<number>.md`.

- When creating files, writing a large string to a single file may fail. The file may be created but its contents are empty. In that case, split the string and divide the write operations into multiple passes to output to a single file.

## Use Case ID
- UC-xxx

## Microsoft Azure resources to be reviewed
- Resource group name: `dahatake{YYYYMMDD}`

## Reference Documents
- docs/usecase/{Use Case ID}/usecase-description.md
- docs/usecase/{Use Case ID}/service-catalog.md
- docs/usecase/{Use Case ID}/AzureServices-services.md
- docs/usecase/{Use Case ID}/AzureServices-data.md
- docs/usecase/{Use Case ID}/AzureServices-services-additional.md

## Location to save review results
- docs/usecase/{Use Case ID}/Azure-ArchitectureReview-Report.md
```

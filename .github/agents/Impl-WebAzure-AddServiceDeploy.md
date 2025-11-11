---

name: Impl-WebAzure-AddServiceDeploy (Step.5)
description: Create additional services (such as Azure OpenAI) via Azure CLI scripts and register them in the service catalog
tools: ["*"]
------------

# Tasks

* In Microsoft Azure, create the services specified in {Additional Services} below. For the latest and detailed specifications of Microsoft Azure services, be sure to search for information on the MCP Server of {MicrosoftDocs} and consider it thoroughly.

* First, analyze the documentation of {Additional Services} with high accuracy and create a clear and executable action plan that aligns with the objective. Leverage structured thinking, domain knowledge, and user-centered design principles to ensure architectural completeness, clarity, and traceability.

* Service creation on Microsoft Azure will be done using Azure CLI scripts. For the latest and detailed specifications and sample code of Azure CLI, be sure to refer to the MCP Server of {MicrosoftDocs}. Save the created script as a Linux-executable file in {Location to Save Azure CLI Scripts}.

* Execute that script using `Azure MCP Server`.

* Also create an installation script for tools and packages required by the Azure CLI script. Save that file to `infra/{Use Case ID}/create-azure-additional-resources-prep.sh`.

* After successfully creating the Microsoft Azure services, append a table to the Services section of the {Service Catalog} document with the service ID, service name, Azure service name, feature name, feature type, Azure service URL, and service region.

* Append the work progress in Japanese to `work/{Use Case ID}/data-azure-additionalservices-deploy-work-status.md`.

* If the work time exceeds 10 minutes, pause the work and split this task into tasks at 10-minute intervals, then create prompts to execute them as Issues. Append each prompt in Japanese to `work/{Use Case ID}/data-azure-additionalservices-deploy-issue-prompt-<number>.md`.

* When creating files, writing a large string into a single file may fail. Sometimes the file is created but the contents are empty. In such cases, split the string to be written and perform multiple write operations, outputting into a single file.

* Append a Japanese summary of the feature overview and application startup steps to `/README.md`.

## Use Case ID

* {Use Case ID}

## Additional Services

* docs/usecase/{Use Case ID}/AzureServices-services-additional.md

## Location to Save Azure CLI Scripts

* infra/{Use Case ID}/create-azure-additional-resources

## Service Catalog

* docs/usecase/{Use Case ID}/service-catalog.md

## Technical Specifications

* Resource group name: {Resource Group Name}
* Region: Japan East

  * If unavailable, select Japan West, or East Asia, or South East Asia. If none are available, anywhere is acceptable.
* For scale settings, be sure to create with the minimum configuration.

  * If a service supports serverless, select it.
  * If a service supports autoscaling, select it.
* Use {Service Name} as the instance name.
* LLM

  * Azure OpenAI Service
  * Models are the latest from OpenAI, such as GPT-5

# Code of Conduct

* Do not speculate on uncertain information; explicitly state assumptions and preconditions.
* To avoid misunderstanding the user’s intent, seek confirmation before making major decisions.
* Provide alternatives and future expansion options to help facilitate decision-making.
* Always consider the balance of quality, security, cost, and schedule.

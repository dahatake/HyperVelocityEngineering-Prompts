---
name: Impl-WebAzure-DataDeploy (Step.2)
description: Create a datastore with an Azure CLI script and bulk-register sample data
tools: ["*"]
------------

# Tasks

* In Microsoft Azure, create the services specified in {Datastore}. Always search for and carefully consider the latest detailed specifications of Microsoft Azure services in the MCP Server of {MicrosoftDocs}.

* First, perform a high-precision analysis of the {Datastore} document and create a clear, actionable execution plan consistent with its objectives. Use structured thinking, domain knowledge, and user-centered design principles to ensure comprehensive architecture, clarity, and traceability.

* Service creation in Microsoft Azure will be done with Azure CLI scripts. Always refer to the MCP Server of {MicrosoftDocs} for the latest detailed Azure CLI specifications and sample code. Save the created script as a Linux-compatible file in {Azure CLI Script Save Location}.

* Execute the script using the `Azure MCP Server`.

* Also create a script for installing the necessary tools and packages required for the Azure CLI script. Save this file as `infra/{UseCaseID}/create-azure-data-resources-prep.sh`.

* After creating the Microsoft Azure services, convert the sample data into appropriate formats for each Azure service and create a Linux-compatible bulk data registration script, then execute it. Save this script in {Data Registration Script Save Location}.

* Once the creation of Microsoft Azure services succeeds, append the service ID, microservice name, service type, service URL, service ID, and service region to the {Service Catalog} document in table format.

* Append the progress status of the work, in Japanese, to `work/{UseCaseID}/data-azure-deploy-work-status.md`.

* If the work takes more than 10 minutes, pause the work and split this task into tasks to be executed every 10 minutes, and create prompts to run them as Issues. Append each prompt, in Japanese, to `work/{UseCaseID}/data-azure-deploy-issue-prompt-<number>.md`.

* When creating files, writing large strings into a single file may fail. The file may be created but its contents may be empty. In such cases, split the output string into smaller segments and write them to the file in multiple operations.

* Add a Japanese description of the feature overview and application startup procedure to `/README.md`.

## Use Case ID

* {UseCaseID}

## Datastore

* docs/usecase/{UseCaseID}/AzureServices-data.md

## Azure CLI Script Save Location

* infra/{UseCaseID}/create-azure-data-resources.sh

## Data Registration Script Save Location

* data/{UseCaseID}/data-registration-script.sh

## Service Catalog

* docs/usecase/{UseCaseID}/service-catalog.md

## Technical Specifications

* Resource group name: {Resource Group Name}
* Region: Japan East

  * If unavailable, choose Japan West, East Asia, or South East Asia.
* Always use minimum configuration for scale settings.

  * Choose serverless options where available.
  * Choose autoscaling options where available.
* Use {ServiceName} as the instance name.

# Code of Conduct

* Do not guess uncertain information. Clearly state assumptions and prerequisites.
* To avoid misunderstanding user intent, ask for confirmation before major decisions.
* Provide alternative options and future expansion ideas to support decision-making.
* Always consider the balance of quality, security, cost, and schedule.

---
name: Impl-WebAzure-ComputeDeploy (Step.7)
description: Create Azure Functions, deploy the code, set up CI/CD with GitHub Actions, and create unit tests.
tools: ["*"]
---

# Purpose
- Referencing the documents in the {Service Catalog}, deploy code implemented as {Microservice Execution Code} by creating each corresponding Microsoft Azure service.
- Do not modify the existing code.
- Do not recreate Microsoft Azure services that have already been created.
- For creating Microsoft Azure services, create Azure CLI scripts, authenticate them through the `Azure MCP Server`, and then execute them. Always refer to the latest and detailed specifications of Azure CLI in the MCP Server of {MicrosoftDocs}. Save the created scripts to {Azure CLI Script Storage Location} as files that run on Linux.
- Create scripts for installing any tools or packages required for Azure CLI scripts. Save these files to `infra/{UsecaseID}/create-azure-api-resources-prep.sh`.

- Implement continuous delivery. For items that can implement CI/CD from GitHub, create GitHub Actions workflows and deploy them to Microsoft Azure. Always refer to the latest and detailed specifications of GitHub Actions in the MCP Server of {MicrosoftDocs}. For items that cannot implement CI/CD, upload the program code to the respective Microsoft Azure services you created.

- After successfully creating Azure services, add the service ID, microservice name, Azure service name, service type, service URL, service ID, and service region to the {Service Catalog} document in table format.

- Create unit test program code for all services running on Azure. For unit tests that call the APIs deployed to Azure Functions, create simple web pages that allow input of API parameters and display the API return values. Save the unit test code in the {Unit Test Creation Folder}.

- Add progress updates in Japanese to `work/{UsecaseID}/api-azure-deploy-work-status.md`.

- If the work time exceeds 10 minutes, stop the work, divide this task into 10-minute tasks, and create Prompts for executing them as Issues. Add each Prompt in Japanese to `work/{UsecaseID}/api-azure-deploy-issue-prompt-<number>.md`.

- When creating files, writing large strings into a single file may fail. Files may be created but end up empty. In such cases, split the string into smaller chunks and write them in multiple operations into the same file.

- Add an overview of functions and application startup instructions in Japanese to `/README.md`.

## Use Case ID
- {UsecaseID}

## Microservice Execution Code
- api/{UsecaseID}/{ServiceID}-{ServiceName}/
  - {ServiceID} is assigned to each service
  - {ServiceName} is the name of each service

## Azure CLI Script Storage Location
- infra/{UsecaseID}/create-azure-api-resources.sh

## Service Catalog
- docs/usecase/{UsecaseID}/service-catalog.md

## Unit Test Creation Folder
- test/{UsecaseID}

## Technical Specifications
- Resource group name: `{UsecaseID}`
- Region: Japan East
  - If unavailable, choose Japan West, East Asia, or South East Asia.

# Code of Conduct
- Do not assume uncertain information; clarify assumptions and prerequisites.
- Request confirmation before any critical decision to avoid misunderstanding user intent.
- Provide alternative options and future expansion ideas to support decision-making.
- Always consider the balance of quality, security, cost, and schedule.

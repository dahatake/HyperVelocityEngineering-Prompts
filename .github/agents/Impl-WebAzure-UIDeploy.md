---
name: Impl-WebAzure-UIDeploy (Step.8)
description: Deploy the web application to Azure Static Web Apps and implement continuous delivery with GitHub Actions.
tools: ["*"]
---

# Tasks
- Deploy the web application code to Microsoft Azure `Azure Static Web Apps`.
- For creating the `Azure Static Web Apps` service, create an Azure CLI script and execute it. Always refer to the MCP Server in {MicrosoftDocs} for the latest and detailed specifications of Azure CLI. Save the created script as a Linux-compatible file in {Azure CLI script storage location}.  
- Create an installation script for tools and packages required by the Azure CLI script. Save this file at `infra/{UseCaseID}/create-azure-webui-resources-prep.sh`.

- Implement continuous delivery. Create a GitHub Actions workflow and deploy to Azure Static Web Apps. Always refer to the MCP Server in {MicrosoftDocs} for the latest and detailed specifications of GitHub Actions.

- After successfully creating the Azure services, add the web application URL to the document in {Service Catalog}.

- Append the work progress in Japanese to `work/{UseCaseID}/screen-azure-deploy-work-status.md`.

- If the work time exceeds 10 minutes, interrupt the work, split this task into 10-minute tasks, and create prompts for executing them as Issues. Append each prompt in Japanese to `work/{UseCaseID}/screen-azure-deploy-issue-prompt-<number>.md`.

- When creating files, write operations may fail if a large string is written to a single file. Files may be created but their contents may be empty. In such cases, split the output string and perform multiple write operations to produce the output in a single file.

- Add a feature overview and application startup instructions in Japanese to `/README.md`.

## Use Case ID
- {Usecase ID}

## Azure CLI Script Storage Location
- infra/{UseCaseID}/create-azure-webui-resources.sh

## Service Catalog
- docs/usecase/{UseCaseID}/service-catalog.md

## Technical Specifications
- Resource group name: `{UseCaseID}`
- Region: East Asia  
  - If unavailable, select Japan West or East Asia or South East Asia.
- Configure at minimum scale.
  - If a service supports serverless, select that option.
  - If a service supports autoscale, select that option.

# Code of Conduct
- Do not guess uncertain information; explicitly state assumptions and prerequisites.
- To avoid misunderstanding the user's intent, seek confirmation before major decisions.
- Provide alternatives and future extension ideas to support decision-making.
- Always consider the balance of quality, security, cost, and schedule.

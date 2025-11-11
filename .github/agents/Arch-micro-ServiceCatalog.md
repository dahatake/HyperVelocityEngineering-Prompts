---
name: Arch-micro-ServiceCatalog (Step.7)
description: Create a mapping table of screens, functions, APIs, and data to clarify the overall picture of the microservice architecture
tools: ["*"]
------------

# Purpose

Analyze and examine all documents in {Reference Documents} in depth, and create a concrete and clear table that catalogs:
Screen → **Functions within the screen → In-screen processing or API call → Data managed by the API**
for the application built with the microservice architecture for this use case.
Be sure to design according to the principles of microservice architecture.

## Requirements:

* For each screen, organize the following:

  * Screen ID
  * Screen name
  * Main functions within the screen (e.g., search, register, delete, etc.)
  * Specify whether each function is “in-screen processing” or an “API call”
  * If it is an API call, provide API ID, API name, endpoint, HTTP method, and key parameters
  * Clearly state the data (SoT) managed by the API
* At the end, also add the responsibilities of API services and managed data

Additionally, if possible, include the following:

* A high-level mapping table that provides an overview of the relationship between screens and APIs

* Best practices and considerations in design (idempotency, concurrency control, bulk operations, etc.)

* If the work time exceeds 10 minutes, stop the work and split this task into tasks in 10-minute increments, and create prompts to execute them as Issues.
  Append each prompt in Japanese to `work/{Use Case ID}/service-catalog-issue-prompt-<number>.md`.

* When creating files, if writing a large string into a single file fails, the file may be created but its contents may be empty. In that case, split the output into multiple write operations and write them incrementally into a single file.

## Use Case ID

* {Use Case ID}

## Reference Documents

* docs/usecase/{Use Case ID}/usecase-description.md
* docs/usecase/{Use Case ID}/services/service-list.md
* docs/usecase/{Use Case ID}/data-model.md
* docs/usecase/{Use Case ID}/screen/screen-list.md

## Output File

* docs/usecase/{Use Case ID}/service-catalog.md

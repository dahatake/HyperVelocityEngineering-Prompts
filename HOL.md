# Hands-on

We will execute prompts for characteristic steps.

# Environment Setup

Here, we will use two Deep Research–type Copilots. This is to ensure they can be used safely and securely in business environments.

* Business Engineering

  * Microsoft 365 Copilot: **Research Tool**

    * Requires Microsoft 365 license plus an additional Microsoft 365 Copilot license.
      [https://www.microsoft.com/ja-jp/microsoft-365-copilot/pricing](https://www.microsoft.com/ja-jp/microsoft-365-copilot/pricing)
    * If the Research Tool does not appear in the Microsoft 365 Copilot Chat screen for your account, you must **contact your administrator** to enable it.
      [https://learn.microsoft.com/ja-jp/copilot/agents?toc=%2Fcopilot%2Fmicrosoft-365%2Ftoc.json&bc=%2Fcopilot%2Fmicrosoft-365%2Fbreadcrumb%2Ftoc.json](https://learn.microsoft.com/ja-jp/copilot/agents?toc=%2Fcopilot%2Fmicrosoft-365%2Ftoc.json&bc=%2Fcopilot%2Fmicrosoft-365%2Fbreadcrumb%2Ftoc.json)

* Software Engineering

  * GitHub Copilot **Coding agent**

    * Requires GitHub Copilot Pro, GitHub Copilot Pro+, GitHub Copilot Business, or GitHub Copilot Enterprise plan.
      [https://docs.github.com/ja/copilot/concepts/agents/coding-agent/about-coding-agent](https://docs.github.com/ja/copilot/concepts/agents/coding-agent/about-coding-agent)
    * If Copilot does not appear after logging into GitHub with your account, you must **contact your administrator** to enable it.

* GitHub Desktop or Visual Studio Code
  Either is fine. Use whichever tool you are accustomed to.

  `GitHub Desktop` is used to synchronize GitHub repositories with folders on your local PC. Download and install it from the link below:

  [https://github.com/apps/desktop](https://github.com/apps/desktop)

  Code editors like `Visual Studio Code` can also sync GitHub repositories with local PC folders. In that case, install the `GitHub extension`.

# Combined Batch and Interactive Processing

We will combine batch-type prompts for large-scale operations and interactive prompts for deep exploration.

For the amount done here, any tool would work — but please separate the tools as follows:

* Batch = GitHub Copilot Coding agent
* Interactive = GitHub Copilot Agent Mode (Plan Mode)

## 1. Creating Multiple Screens – Batch Type

From this prompt, we will generate the hands-on sample code and the prompts. 
The generated text will be used as **the hands-on material**. Please read it carefully and proceed with the hands-on.


```text
# Purpose
Design a process using GitHub Copilot to generate HTML and JavaScript programs for prompt art samples, and then execute program modification, bug detection, and fix proposals through both batch-type and interactive-type prompts.

## Workflow
1. Requirements Definition
  - HTML will have two screens (Builder screen / Analyzer screen).
  - Shared JS to share logic between both screens.
  - Prepare prompt sets used for generative AI (creation, diagramming, bug identification & fix proposals).
  - Embed intentional bugs in the code (for learning purposes).
2. Design Guidelines
  - Builder screen: Create “prompts” from text + selection UI, draw a simple preview on Canvas, and save to LocalStorage.
  - Analyzer screen: Load saved prompts, list/display and analyze them (but includes intentional bugs that prevent some functionality).
  - JS will be one file (app.js) with modular structure.
  - Bugs will be concentrated on the Analyzer side; Builder should mostly work to simplify learning.
3. Intentional Bug Placement
  - Bug #1 (Storage key mismatch): Saving uses `prompts`, loading uses `promptList` → nothing appears in Analyzer.
  - Bug #2 (Button ID mismatch): Analyzer’s event registration refers to `analyzeButton` while HTML uses `analyzeBtn` → click not detected.
4. Prompt Design
  - Program creation prompt: A single prompt that makes Copilot generate two screens + shared JS per the specification.
  - Batch-type prompt: Generates mass deliverables (test cases, issue tickets, log design, diagrams) in bulk.
  - Interactive ①: Diagramming code (Mermaid) + structural explanation in Japanese.
  - Interactive ②: From bug-report text, propose root cause, code location, fix patches, and reproduction & verification steps.
5. Delivery & Usage Instructions
  - First distribute the sample code and verify operation.
  - Next, use Interactive ① for structural understanding, then Interactive ② for bug-fix exercises.
  - Use batch-type prompts to generate large sets of deliverables and compile workshop outcomes.

If the program itself or the code to be analyzed is incomplete or missing, state so explicitly and output an appropriate error message along with analysis/fix prompts.

- All code examples must be output in Markdown code block format.
- Batch-type and interactive-type must include separate explanations and example prompts.
- Bug reports and fix proposals must be listed in specific detail.
- Include output samples for cases where the analysis target is missing or incomplete.

### Output Verbosity
- Explanations and example prompts should be 1–2 sentences or 2 paragraphs, up to 6 lines maximum — concise but clear.
- Provide necessary and sufficient content with strong structure and specificity.
- Always return complete and practical responses within the specified length.
- Even if the user gives short instructions, provide complete answers without omitting required details.
- Do not increase length for politeness (“Do not increase length to restate politeness.”).
```

# 2. Large-Scale Batch Processing with AI First

## 1. Creating Business Requirements: Microsoft 365 Copilot

* Log into Microsoft 365 Copilot Chat.

  [https://m365.cloud.microsoft/chat](https://m365.cloud.microsoft/chat)

* Open Notepad or another text editor.
  It is convenient to edit and save prompts in a text editor before submitting them.

* Copy the following prompt and paste it into the text editor:
  [Prompt](Business-Documentation.md#step-1-事業分析ドキュメントの作成)

* Insert the following content between `目的` (Purpose) and `ガイドライン` (Guidelines):

```text
# Service Planning

A “loyalty program” is a mechanism where a company offers points or rewards based on customer purchases or usage, encouraging customers to repeatedly use the company’s products or services.

Key features include:
1. Increasing customer loyalty: Encourages repeat use and builds long-term relationships.
2. Collecting customer data and personalization: Enables tailored rewards using purchase history and behavioral data.
3. Competitive advantage in business: Differentiates a company by increasing customer loyalty through points or reward systems.

Example personas may include:
- Frequent customers who regularly use the same store or brand.
- New customers who are sensitive to points or rewards.
- Families that regularly use services or products.

Use of generative AI:
- Propose personalized rewards using natural language processing from customer interactions.
- Use voice input and commands to support smooth operations even with fewer staff.
- Predict next purchases using purchase data and automatically deploy optimal campaigns.
```

* Paste the edited prompt into Microsoft 365 Copilot Chat and execute.

> [!TIP]
> Time for coffee or tea. It will take **5–10 minutes** for Copilot to finish. However, you may **immediately** open a new browser tab or a new instance and proceed to the next step.

* After execution, a long document will be generated. Click the `Copy response` button at the very end of the output, copy everything, and paste it into a text editor. Save it on your PC as a **Markdown file** named `business-requirement.md`.

## 2. Creating a New Repository on GitHub

Log into GitHub and create a new repository.

* Repository name is optional — e.g., `hve-hol`.
* Visibility is recommended to be **Private**.

## 3. Uploading Sample Requirement Definition Documents

Upload all folders and files inside the `samples` folder to the new GitHub repository.

`samples` folder structure:

```text
samples
 ├── .github/agents           : GitHub Copilot Custom Agent definitions
 │    ├── Arch-micro-ServiceIdentify.md
 │    ├── Arch-UI-Detail
 │    └── Impl-WebAzure-UICoding
 ├── data/sample-data.json    : Sample data
 ├── docs                     : Sample requirement and design documents
 │    ├── business-requirement.md
 │    ├── domain-analytics.md
 │    └── ...
 └── .gitignore
```

Sync your local PC folder and GitHub repository using GitHub Desktop or Visual Studio Code.

If Step 1 is complete, replace `business-requirement.md` with the one you created.

## 4. Microsoft Learn MCP Server Settings

Configure the system to retrieve detailed information about C# and Microsoft Azure from Microsoft Learn.

> [!WARNING]
> Carefully review the usage conditions for Microsoft Learn MCP Server:
> [https://learn.microsoft.com/ja-jp/training/support/mcp#requirements](https://learn.microsoft.com/ja-jp/training/support/mcp#requirements)

* Navigate to your GitHub repository.
* Select the `Settings` tab.
* From the left menu, choose `Copilot` > `Coding agent`.
* In the `MCP Conditions` section, paste the following:

```text
{
  "mcpServers": {
    "MicrosoftDocs": {
      "type": "http",
      "url": "https://learn.microsoft.com/api/mcp",
      "tools": ["*"]
    }
  }
}
```

## 5. Microservice Modeling

Using the sample files as reference, submit the following prompt to GitHub Copilot Coding agent to perform microservice modeling.

* Navigate to your GitHub repository.

* Open the `Issue` tab.

* Click `New issue`.

* Enter the following into each field:

  * **Title:** マイクロサービスのモデリング (Microservice Modeling)
  * **Description:** Paste the following:

  ```text
  Refer to the documents listed in {Reference Documents} and perform microservice modeling. Save the modeling results in Markdown format into {Output File}.

  ## Reference Documents
    - docs/usecase-description.md
    - docs/domain-analytics.md

  ## Output File
    - docs/service-list.md
  ```

* Under `Assignees`, select **Copilot**.

* You will be able to select a **Custom Agent**. Choose **Arch-micro-ServiceIdentify**.

  ![Selecting Custom Agent](./images/assign-githubcopilot-customagent.jpg)

* Click the green `Create` button.

> [!TIP]
> Time for coffee or tea. It will take **5–10 minutes** for Copilot to finish, but you may **immediately** proceed to the next step in another tab.

Reference:
[https://docs.github.com/ja/copilot/how-tos/use-copilot-agents/coding-agent/create-a-pr](https://docs.github.com/ja/copilot/how-tos/use-copilot-agents/coding-agent/create-a-pr)

* Open the Pull Request linked to the Issue and check the results. If you see **Copilot finished work on behalf of <UserName>**, the task is complete.
* If the Pull Request shows a `view sessions` button, click it to review **how Copilot performed the work**.
* Use the `File changed` tab to review added/modified/deleted files.
* If satisfied, click `Ready for review`.
* Merge the Pull Request.

## 6. Designing Azure Data Services

Select the optimal Microsoft Azure data services considering each microservice’s workload.

* Create a new Issue on GitHub.

* Enter the following:

  * **Title:** Microsoft Azureのデータサービスの選定 (Selecting Azure Data Services)
  * **Description:** Paste:

  ```text
  Refer to the documents listed in {Reference Documents} and select the optimal Microsoft Azure data services. Save the analysis results in Markdown format into {Output File}.

  ## Reference Documents
    - docs/usecase-description.md
    - docs/data-model.md
    - docs/service-list.md

  ## Output File
    - docs/AzureServices-data.md
  ```

* Assign **Copilot**.

* Choose Custom Agent: **Impl-WebAzure-DataDesign**.

* Click `Create`.

* After Copilot completes the Pull Request, review and merge.

## 7. Creating Screen Definition Document

We will design and implement one screen. This can be done right after Step 5 or Step 6.

* Create a new Issue.

* Enter:

  * **Title:** 画面定義書の作成 (Screen Definition Document)
  * **Description:** Paste:

  ```text
  Refer to the documents listed in {Reference Documents} and create the screen definition document specified in {Target Screen}. Use the data from {Sample Data File} in the screen. Save the screen definition document in Markdown format into {Output File}.

  ## Target Screen
    - SCR-006-001: Portal Screen

  ## Reference Documents
    - docs/usecase-description.md
    - docs/service-list.md
    - docs/data-model.md

  ## Output File
    - docs/UI-SCR-006-001.md

  ## Sample Data File
    - data/sample-data.json
  ```

* Assign **Copilot**.

* Choose Custom Agent: **Arch-UI-Detail**.

* Click `Create`.

* Review and merge the Pull Request after completion.

## 8. Screen Coding

Code the screen based on the screen definition document created in Step 7.

* Create a new Issue.

* Enter:

  * **Title:** Web画面の実装 (Web Screen Implementation)
  * **Description:** Paste:

  ```text
  Refer to the documents listed in {Reference Documents} and implement the Web screen specified in {Target Screen}. Use the data from {Sample Data File} within the screen. Save the created code into {Output Folder}.

  ## Target Screen
    - docs/UI-SCR-006-001.md

  ## Reference Documents
    - docs/usecase-description.md
    - docs/service-list.md
    - docs/data-model.md

  ## Technical Specifications
    - HTML5 and JavaScript

  ## Output Folder
    - apps
  ```

* Assign **Copilot**.

* Choose Custom Agent: **Arch-UI-Detail**.

* Click `Create`.

* Review and merge the Pull Request after completion.

## 8. Operation Verification

The created screen is HTML only. Download it locally and open it in a web browser.

Other outputs are documents only. Check them on GitHub.

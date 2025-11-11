# Hands-on

We will perform Prompts for characteristic steps in entire Hypervelocity Engineering process.

# Environment Setup

Here, we will use two types of Deep Research Copilots. This is to ensure that they can be used safely and securely in business operations.

* Business Engineering

  * Microsoft 365 Copilot: **Researcher**

    * Requires a Microsoft 365 license and an additional Microsoft 365 Copilot license.
      [https://www.microsoft.com/en-us/microsoft-365-copilot/pricing](https://www.microsoft.com/en-us/microsoft-365-copilot/pricing)
    * If the researcher does not appear in the Microsoft 365 Copilot Chat screen for your account, you need to **contact your administrator** to enable it.
      [https://learn.microsoft.com/en-us/copilot/agents?toc=%2Fcopilot%2Fmicrosoft-365%2Ftoc.json&bc=%2Fcopilot%2Fmicrosoft-365%2Fbreadcrumb%2Ftoc.json](https://learn.microsoft.com/en-us/copilot/agents?toc=%2Fcopilot%2Fmicrosoft-365%2Ftoc.json&bc=%2Fcopilot%2Fmicrosoft-365%2Fbreadcrumb%2Ftoc.json)

* Software Engineering

  * GitHub Copilot **Coding agent**

    * Requires GitHub Copilot Pro, GitHub Copilot Pro+, GitHub Copilot Business, or GitHub Copilot Enterprise plan.
      [https://docs.github.com/ja/copilot/concepts/agents/coding-agent/about-coding-agent](https://docs.github.com/ja/copilot/concepts/agents/coding-agent/about-coding-agent)
    * If Copilot does not appear when logged into your GitHub account, you need to **contact your administrator** to enable it.

* GitHub Desktop or Visual Studio Code

  Either is fine. Use whichever tool you are comfortable with.

  `GitHub Desktop` is used to synchronize GitHub repositories with local PC folders. Download and install it from the link below:

  [https://github.com/apps/desktop](https://github.com/apps/desktop)

  Code editors such as `Visual Studio Code` can also sync GitHub repositories with local PC folders. In that case, install the `GitHub extension`.

# Procedure

## 1. Creating Business Requirements: Microsoft 365 Copilot

* Log in to Microsoft 365 Copilot Chat.

  [https://m365.cloud.microsoft/chat](https://m365.cloud.microsoft/chat)

* Open a text editor such as Notepad. It is convenient to edit and save the Prompt in a text editor before inputting it.

* Copy the following Prompt content and paste it into the text editor.
  [Prompt](Business-Documentation.md#step-1-事業分析ドキュメントの作成)

* Add the following content **between** `Purpose` and `Guidelines`.

```text
# Service Planning

First, a “loyalty program” is a mechanism in which companies provide points or benefits based on purchases or usage to encourage customers to continue using their products or services.

As for the *features*:
1. Enhances customer loyalty by encouraging repeat use and building long-term relationships.  
2. Accumulates customer data and enables personalization. Based on purchase history and behavioral data, optimal benefits can be offered to each customer.  
3. Creates a competitive advantage in business by boosting customer loyalty through benefits and point systems, differentiating from competitors.

As for the *persona*, the following customer segments may be assumed:  
- Regular customers who frequently use the same store or brand.  
- New customers who are particularly sensitive to points or benefits.  
- Families who regularly use certain services or products.

As for the *use of generative AI*:  
- Use natural language processing to propose personalized benefits based on interactions with customers.  
- Use voice input or voice commands to improve operations even with fewer staff.  
- Additionally, AI can predict the next purchase based on purchase data and automatically run optimal campaigns.
```

* Paste the edited Prompt into Microsoft 365 Copilot Chat and execute it.

> [!TIP]
> Time for coffee or tea. Copilot will take **5–10 minutes** to finish. However, you can **immediately** open a new browser tab or launch another instance to proceed to the next step.

* After execution, a fairly long text will be generated. Click the `Copy response` button at the very bottom to copy the entire output. Paste that text into a text editor and save it as a **Markdown file** named `business-requirement.md` on your PC.

## 2. Creating a New Repository on GitHub

Log in to GitHub and create a new repository.

* The repository name can be anything. For example, `hve-hol`.
* It is recommended to set Visibility to **Private**.

## 3. Uploading Sample Requirement Definition and Documentation Files

Upload all folders and files under the `samples` folder into the new GitHub repository.

`samples` folder structure:

```text
samples
 ├── .github/agents           : Custom Agent definitions for GitHub Copilot
 │    ├── Arch-micro-ServiceIdentify.md
 │    ├── Arch-UI-Detail
 │    └── Impl-WebAzure-UICoding
 ├── data/sample-data.json    : Sample data
 ├── docs                     : Sample requirement definitions and design documents
 │    ├── business-requirement.md
 │    ├── domain-analytics.md
 │    └── ...
 └── .gitignore
```

Use GitHub Desktop or Visual Studio Code to synchronize the local PC folder and the GitHub repository.

If Step 1 has already been completed, replace `business-requirement.md` with the one you created.

## 4. Microsoft Learn MCP Server Settings

Configure settings so that detailed information about C# and Microsoft Azure can be obtained from Microsoft Learn.

> [!WARNING]
> Please carefully review the usage conditions for the Microsoft Learn MCP Server:
> [https://learn.microsoft.com/en-us/training/support/mcp#requirements](https://learn.microsoft.com/en-us/training/support/mcp#requirements)

* Go to the repository you created on GitHub.
* Select the `Settings` tab.
* From the left menu, select `Copilot` > `Coding agent`.
* In the `MCP Conditions` section, paste the following text:

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

Referencing the sample files, send the following Prompt to the GitHub Copilot Coding agent to perform microservice modeling.

* Go to your GitHub repository

* Select the `Issues` tab

* Click `New issue` to create a new issue

* Enter the following information:

  * Title: Microservice Modeling
  * Add a description: Copy and paste the following Prompt:

  ```text
  Please perform microservice modeling referring to the documents in {reference documents}. Save the modeling results in {output file} in Markdown format.

  ## Reference Documents
    - docs/usecase-description.md
    - docs/domain-analytics.md

  ## Output File
    - docs/service-list.md
  ```

* Under `Assignees`, select **Copilot**.

* You can select a `Custom Agent`. Choose **Arch-micro-ServiceIdentify**.

  ![Selecting Custom Agent](./images/assign-githubcopilot-customagent.jpg)

* Click the green `Create` button.

> [!TIP]
> Time for coffee or tea. Copilot will take **5–10 minutes** to finish. However, you can **immediately** open a new browser tab or launch another instance to proceed to the next step.

Reference:
[https://docs.github.com/ja/copilot/how-tos/use-copilot-agents/coding-agent/create-a-pr](https://docs.github.com/ja/copilot/how-tos/use-copilot-agents/coding-agent/create-a-pr)

* Display the `Pull Request` linked to the Issue and check the results. If it shows **Copilot finished work on behalf of <username>**, the task is complete.
* If there is a `view sessions` button in the Pull Request, click it to check the details. You can see **what work Copilot performed**.
* In the `File changed` tab, you can review created, modified, or deleted files.
* If everything looks good, press `Ready for review`.
* Then merge.

## 6. Designing Azure Data Services

Select Microsoft Azure data services considering the workload of each microservice.

* Create a new Issue in GitHub

* Enter the following information:

  * Title: Selection of Microsoft Azure Data Services
  * Add a description: Copy and paste the following Prompt:

  ```text
  Please select the optimal data services in Microsoft Azure, referring to the documents in {reference documents}. Save the analysis results in {output file} in Markdown format.

  ## Reference Documents
    - docs/usecase-description.md
    - docs/data-model.md
    - docs/service-list.md

  ## Output File
    - docs/AzureServices-data.md
  ```

* Under `Assignees`, select **Copilot**.

* Choose **Impl-WebAzure-DataDesign** as the `Custom Agent`.

* Click the green `Create` button.

* After Copilot completes the Pull Request, review and merge.

## 7. Creating a Screen Definition Document

We will design and implement only one screen. You may do this immediately after Step 5 or Step 6.

* Create a new Issue in GitHub

* Enter the following information:

  * Title: Creating Screen Definition Document
  * Add a description: Copy and paste the following Prompt:

  ```text
  Please create the screen definition document for the screen specified in {target screen}, referring to the documents in {reference documents}. Use data from {sample data file} within the screen. Save the screen definition document in {output file} in Markdown format.

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

* Choose **Arch-UI-Detail** as the Custom Agent.

* Click the green `Create` button.

* After Copilot completes the Pull Request, review and merge.

## 8. Screen Coding

Perform screen coding based on the screen definition created in Step 7.

* Create a new Issue in GitHub

* Enter the following information:

  * Title: Web Screen Implementation
  * Add a description: Copy and paste the following Prompt:

  ```text
  Please create the Web screen code specified in {target screen}, referring to the documents in {reference documents}. Use data from {sample data file} within the screen. Save the created code in {output folder}.

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

* Choose **Arch-UI-Detail** as the Custom Agent.

* Click the green `Create` button.

* After Copilot completes the Pull Request, review and merge.

## 8. Operation Check

The screen created is HTML only. You can download it locally and display it in a web browser.

For everything else, only documentation is created. Please check the content on GitHub.


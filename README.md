# Multi-Agent Customer Feedback Analysis Workflow

This project demonstrates a sequential multi-agent workflow using the
Microsoft Agent Framework and a Microsoft Foundry model. It reads customer
reviews from a CSV file, passes them through three specialized agents, and
saves one consolidated report as a Markdown file. Intermediate
agent responses are not printed or saved.

## How it works

The Python script loads the CSV rows and sends them together to a workflow
created with `SequentialBuilder`:

```text
CSV reviews -> Summarizer -> Classifier -> Action -> Final report
```

- **Summarizer** highlights positive feedback, recurring problems, and feature
  requests, using review IDs as evidence.
- **Classifier** organizes the findings by sentiment and theme, and separates
  recurring patterns from individual comments.
- **Action** combines the previous findings into a Markdown report with an
  executive summary, recommended actions, and data limitations.

The workflow runs once for the entire CSV. The model generates the analysis and
recommendations; it does not calculate statistically guaranteed counts. Large
CSV files may exceed the model's context window and need a batched workflow.

## Requirements

- Python 3.11
- Azure CLI
- A Microsoft Foundry project with a deployed model
- Access to that project for the Azure CLI account used to sign in

## Run locally (PowerShell)

Run these commands from the repository folder to create a virtual environment
and install the Python dependencies:

```powershell
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Sign in with the Azure CLI. The app uses `AzureCliCredential` to authenticate:

```powershell
az login
```

Create a `.env` file in the repository root with the Foundry project endpoint
and the name of the deployed model:

```dotenv
AZURE_AI_PROJECT_ENDPOINT=https://<your-foundry-project-endpoint>
AZURE_AI_MODEL_DEPLOYMENT_NAME=<your-model-deployment>
```

Run the sample dataset, which contains 20 fictional English reviews:

```powershell
python agents.py reviews.csv
```

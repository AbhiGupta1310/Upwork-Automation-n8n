# Local Setup Guide: Upwork Job Scraper & Scorer

This guide explains how to set up and run the **Updated Upwork Automation** workflow in your local n8n instance.

## Prerequisites

- **n8n**: Installed and running locally (via `npm`, Docker, or Desktop app).
- **Accounts**:
  - **Apify**: Account with the `neatrat/upwork-job-scraper` actor ready.
  - **OpenAI**: API Key (gpt-5-mini / gpt-4o access).
  - **Airtable**: account with a Base and Table set up.
  - **Gmail**: Google account for sending alerts.

## 1. Import Workflow

1.  Open your local n8n dashboard (`http://localhost:5678`).
2.  Click **Workflows** > **Add Workflow**.
3.  Click the **three dots** (top right) > **Import from File**.
4.  Select `Updated Upwork Automation.json`.

## 2. Configure Credentials

You need to set up 4 credentials in n8n for this workflow to run.

### A. Apify (HTTP Request)

The workflow uses Generic Authentication.

1.  Double-click the **HTTP Request** node.
2.  Under **Authentication**, it should show "Generic Credential Type".
3.  Select both **Header Auth** and **Query Auth** if prompted, or just **Header Auth** (Recommended).
4.  Create a new credential:
    - **Name**: `Apify API`
    - **Header Name**: `Authorization`
    - **Value**: `Bearer YOUR_APIFY_TOKEN`
    - _(Alternatively, if using Query Auth, key=`token`, value=`YOUR_APIFY_TOKEN`)_.

### B. OpenAI

1.  Double-click the **OpenAI Chat Model** node.
2.  Under **Credential to connect with**, select **Create New Credential**.
3.  Choose **OpenAI API**.
4.  Enter your **API Key**.

### C. Airtable

1.  Double-click the **Create a record** node.
2.  Under **Credential to connect with**, select **Create New Credential**.
3.  Choose **Airtable Personal Access Token API**.
4.  Enter your **Access Token**.

### D. Gmail (OAuth2)

1.  Double-click the **Email alert** node.
2.  Under **Credential to connect with**, select **Create New Credential**.
3.  Choose **Gmail OAuth2 API**.
4.  Follow the [n8n Gmail Setup Guide](https://docs.n8n.io/integrations/builtin/credentials/google/gmail/) to generate your Client ID and Secret from Google Cloud Console.
    - _Note: For local setup, this can be tricky with redirect URLs. Ensure your redirect URI matches your local n8n config._

## 3. Workflow Configuration

### Verify IDs and Emails

1.  **Airtable Node**:
    - The Base ID (`appy5w...`) and Table ID (`tbl9p...`) are currently hardcoded in the updated file.
    - **Action**: If these are your correct IDs, keep them. If not, update them in the **Create a record** node settings.
2.  **Gmail Node**:
    - **To Email**: Currently set to `iamabhids@gmail.com`.
    - **Action**: Change this if you want alerts sent to a different address.

## 4. Testing

1.  Click **Execute Workflow** (bottom header).
2.  Watch the execution.
    - **HTTP Request**: Should fetch jobs from Apify.
    - **Filter**: Should remove low-quality jobs.
    - **OpenAI**: Should score the remaining jobs.
    - **Airtable**: Should add records.

## Troubleshooting

- **Apify Error**: Check your token and ensure you have runs available.
- **OpenAI Error**: Check your API key and credit balance.

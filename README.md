# Meeting Minutes Generator - Azure Logic App

This repository contains Infrastructure as Code (IaC) for deploying an automated meeting minutes generator using Azure Logic Apps.

## Overview

This Logic App workflow automates the process of converting meeting transcripts into professional meeting minutes using Azure OpenAI. The workflow monitors an Azure Blob Storage container for new transcript files, processes them through Azure OpenAI's GPT-4 model, and saves the generated meeting minutes to SharePoint Online.

## Architecture

The solution uses the following Azure services:

- **Azure Logic Apps**: Orchestrates the workflow
- **Azure Blob Storage**: Stores meeting transcript files
- **Azure OpenAI**: GPT-4 model for generating meeting minutes
- **SharePoint Online**: Stores the generated meeting minutes
- **Managed Identity**: Provides secure authentication to Azure services

## Workflow Steps

1. **Trigger**: Monitors Azure Blob Storage (`/files` folder) every minute for new or updated files
2. **Get Blob Content**: Retrieves the content of the uploaded transcript file
3. **Parse Document**: Extracts text from the document
4. **Generate Meeting Minutes**: Sends the transcript to Azure OpenAI (GPT-4) with a prompt to generate structured meeting minutes including:
   - Title of meeting
   - Date
   - Attendees
   - Agenda items discussed
   - Decisions made
   - Action items with owners and due dates
5. **Save to SharePoint**: Creates a new text file in SharePoint Online with the generated meeting minutes

## Prerequisites

Before deploying this Logic App, ensure you have:

- An active Azure subscription
- An Azure Blob Storage account with a container named `lahellologicapps` and a `/files` folder
- An Azure OpenAI resource with a `gpt-4` deployment
- A SharePoint Online site with appropriate permissions
- API connections configured for:
  - Azure Blob Storage (with Managed Service Identity authentication)
  - Azure OpenAI (with Managed Service Identity authentication)
  - SharePoint Online

## Deployment

Click the button below to deploy this Logic App to your Azure subscription:

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FDougChisholm%2FHelloLogicAppIaCBasic%2Fmain%2Flogicapp.json)

Alternatively, you can deploy using Azure CLI or PowerShell with the provided templates:

- **ARM Template**: `logicapp.json`
- **Bicep Template**: `logicapp.bicep`

### Deployment Parameters

You'll need to provide the following parameters during deployment:

- `workflows_laConsAITest_name`: Name of the Logic App (default: `laConsAITest02`)
- `connections_azureblob_1_externalid`: Resource ID of the Azure Blob Storage API connection
- `connections_azureopenai_3_externalid`: Resource ID of the Azure OpenAI API connection
- `connections_sharepointonline_1_externalid`: Resource ID of the SharePoint Online API connection

## Usage

1. Upload a meeting transcript file (text or supported document format) to the `/files` folder in your Azure Blob Storage container
2. The Logic App will automatically trigger within 1 minute
3. The transcript will be processed and sent to Azure OpenAI for summarization
4. The generated meeting minutes will be saved to SharePoint Online in the `/SitePages` folder with a unique filename

## Location

The Logic App is deployed in the **UK South** region by default.

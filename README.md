# WhatsApp-Driven Google Drive Assistant (n8n)

This project, created for an internship task, is a fully functional assistant that lets you manage your Google Drive files by sending simple commands through WhatsApp. The entire workflow is built and automated using n8n and is designed to be run with Docker.

### ✨ Demo Output

![WhatsApp Drive Assistant Demo](https://storage.googleapis.com/gemini-generative-ai-public/readme-demo-1/whatsapp-drive-assistant-demo.png)

### ✅ Features

* [cite_start]**List Files:** Get a list of all files in a specified Google Drive folder. [cite: 38]
* [cite_start]**Move Files:** Move any file to a different folder within your Drive. [cite: 40]
* [cite_start]**Delete Files:** Securely delete files with a confirmation step. [cite: 39]
* [cite_start]**AI Summarization:** Get a concise, bullet-point summary of text documents (`.pdf`, `.docx`, `.txt`) using OpenAI. [cite: 41, 42]
* [cite_start]**Audit Logging:** Every command received is automatically logged in a Google Sheet for security and monitoring. [cite: 52]

### 🛠️ Tech Stack

* **Automation:** n8n
* **Containerization:** Docker & Docker Compose
* **Messaging:** Twilio API for WhatsApp
* **Cloud Storage:** Google Drive API
* **Logging:** Google Sheets API
* **AI:** OpenAI Assistants API

### 🚀 Setup Instructions

Follow these steps to get the project running on your local machine.

#### **1. Prerequisites**

Make sure you have the following installed and configured:
* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* A [Twilio](https://www.twilio.com/) account
* A [Google Cloud Platform](https://console.cloud.google.com/) account
* An [OpenAI](https://platform.openai.com/) account with API access

#### **2. Clone the Repository**

```bash
git clone <your-repository-url>
cd <repository-folder>
```

#### **3. Configure Your Environment**

This project uses credentials stored within n8n. You will need to gather the following keys and IDs from the respective services:

* **Twilio:** Account SID & Auth Token.
* **Google Cloud:**
    * OAuth Client ID & Client Secret for Google Drive & Google Sheets APIs.
    * The Spreadsheet ID for your logging sheet.
* **OpenAI:** Your OpenAI API Key.

#### **4. Run with Docker**

First, ensure no other container is using the name `n8n`. If you have an old one, remove it:
```bash
docker stop n8n && docker rm n8n
```
Then, start the project using Docker Compose:
```bash
docker-compose up -d
```
This will start n8n in the background. You can access the canvas at `http://localhost:5678`.

#### **5. Configure Services & Workflow**

1.  **Twilio Sandbox:**
    * Activate the Twilio Sandbox for WhatsApp and link your phone number.
    * In your n8n canvas, copy the **Production Webhook URL**.
    * Paste this URL into the "WHEN A MESSAGE COMES IN" field in your Twilio Sandbox settings.

2.  **Google Credentials:**
    * In the Google Cloud Console, enable the **Google Drive API** and **Google Sheets API**.
    * Create **OAuth 2.0 Credentials**. When asked for the "Authorized redirect URI," copy the one provided from within the n8n Google credential creation screen.

3.  **Google Sheet for Logging:**
    * Create a new Google Sheet.
    * In the first row, create the headers: `Timestamp`, `User`, `Command`, `Path`, `Argument`.
    * Copy the **Spreadsheet ID** from the sheet's URL.

4.  **n8n Workflow Setup:**
    * Import the `workflow.json` file provided in this repository into your n8n canvas.
    * The workflow will show credential errors. Click on each of the following nodes and create/select the appropriate credential:
        * **Twilio Node:** Connect your Twilio credential.
        * **Google Drive Nodes:** Connect your Google Drive OAuth2 credential.
        * **Google Sheets Node:** Connect your Google Sheets credential and paste your Spreadsheet ID.
        * **OpenAI Agent Node ("Message an assistant"):** Connect your OpenAI credential. *Note: You may need to run the `Create an assistant` action once separately to get an Assistant ID to use here.*
    * Activate the workflow using the toggle switch.

### 💬 Command Syntax

You can now send commands from your WhatsApp number to your Twilio number.

| Command | Format | Description |
| :--- | :--- | :--- |
| **LIST** | `LIST /FolderName` | [cite_start]Lists all files in the specified folder. [cite: 38] |
| **DELETE** | `DELETE /path/to/file.pdf CONFIRM` | Deletes a file. [cite_start]The `CONFIRM` keyword is required. [cite: 39] |
| **MOVE** | `MOVE /path/to/file.pdf /destination_folder` | [cite_start]Moves a file to a new folder. [cite: 40] |
| **SUMMARY**| `SUMMARY /path/to/document.txt` | [cite_start]Generates a summary of the specified document. [cite: 41] |

### 📄 License

[cite_start]This project is licensed under the MIT License. [cite: 20]

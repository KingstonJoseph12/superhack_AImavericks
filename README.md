# Automated Incident Analyzer (Streamlit + LangChain + AWS Bedrock)

This project is a **Streamlit-based AI-driven Incident Analyzer** that integrates **LangChain**, **AWS Bedrock**, and **SQLite** to intelligently classify, analyze, and resolve IT incidents.
It uses **Claude 3.5 (Sonnet)** via AWS Bedrock to reason over structured and unstructured data, execute SQL queries dynamically, and generate actionable reports.

---

## Overview

The **Automated Incident Analyzer** serves as a modular framework for intelligent incident management. It:

* Connects to a local or external database (SQLite used here)
* Reads and uses markdown-based knowledge base documents
* Classifies incoming incident descriptions into predefined categories
* Invokes specialized AI agents to:

  * Extract key incident data
  * Perform root-cause analysis
  * Suggest next actions or resolutions
* Displays the entire process interactively using Streamlit

The system is designed to be easily extended with more knowledge bases or LLM backends (e.g., Azure OpenAI, OpenAI GPT models).

---

## Features

1. **Incident Classification**

   * Automatically identifies the type of incident (Monitoring Alert, EBC Incident, Autosys/Kerberos Issue).
2. **Dynamic SQL Agent Integration**

   * Executes database queries using LangChain’s SQL Agent to support incident analysis and verification.
3. **Multi-LLM Support**

   * Currently uses **AWS Bedrock (Claude 3.5 Sonnet)**.
   * Code includes placeholders for **Azure OpenAI** and **OpenAI GPT models**.
4. **Knowledge Base Integration**

   * Reads from multiple markdown files:

     * `Input_Knowledge_base.md`
     * `EBC_Knowledge_base.md`
     * `Kerberos_Knowledge_base.md`
5. **Tool-Enhanced Agents**

   * Uses custom LangChain tools for:

     * Log extraction
     * Kerberos job restarts via ServiceNow templates
6. **Streamlit Interactive UI**

   * Sidebar navigation between:

     * Incident Analyzer
     * Input Knowledge Base
     * EBC Knowledge Base
     * Kerberos Knowledge Base
7. **Automatic Email and Report Generation**

   * Generates formatted reports and assurance emails using AI prompts.

---

## Project Structure

```
.
├── st_aws.py                        # Main Streamlit application (this file)
├── data.db                          # SQLite database used for queries
├── Input_Knowledge_base.md          # Base reference document for classification
├── EBC_Knowledge_base.md            # Reference for contact mismatch incidents
├── Kerberos_Knowledge_base.md       # Reference for Autosys/Kerberos job failures
├── .env                             # Contains API keys and configurations
└── requirements.txt                 # Python dependencies
```

---

## Setup and Installation

### 1. Clone the Repository

```bash
git clone https://github.com/superhack_AImavericks.git
```

### 2. Create a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate       # On macOS/Linux
venv\Scripts\activate          # On Windows
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Environment Variables

Create a `.env` file in the project root:

```bash
AWS_ACCESS_KEY_ID=your_aws_key
AWS_SECRET_ACCESS_KEY=your_aws_secret
AWS_DEFAULT_REGION=us-east-1
# Optional Azure keys if using Azure OpenAI
GPT_4o_KEY=your_azure_api_key
```

### 5. Run the Streamlit App

```bash
streamlit run st_aws.py
```

---

## Knowledge Base Files

Each `.md` file defines incident types and contextual knowledge.

* **Input_Knowledge_base.md**
  Defines incident types and sample classification logic.

* **EBC_Knowledge_base.md**
  Provides structure for analyzing mismatched contact details in customer data.

* **Kerberos_Knowledge_base.md**
  Contains resolution procedures for Autosys and Kerberos-related failures.

---

## Example Workflow

1. **Launch the app**

   ```bash
   streamlit run st_aws.py
   ```
2. **Navigate to “Incident Analyzer”**
3. **Paste an incident description**, for example:

   ```
   EBC reported policy number 12ABCD3456 with incorrect contact email in system.
   ```
4. **Click “Submit”**
5. The AI:

   * Classifies the incident type (EBC_Incident)
   * Extracts structured details from the text
   * Queries the local database for reference information
   * Summarizes analysis and generates a report
6. Download the AI-generated **response summary** directly from the app.

---

## Technical Components

| Component                           | Description                                                                 |
| ----------------------------------- | --------------------------------------------------------------------------- |
| **Streamlit**                       | UI framework for displaying the application                                 |
| **LangChain**                       | Framework for creating agents, toolkits, and workflows                      |
| **AWS Bedrock (Claude 3.5 Sonnet)** | Core LLM for reasoning and generation                                       |
| **SQLite**                          | Lightweight local database for SQL Agent integration                        |
| **Markdown Files**                  | Used as domain-specific knowledge bases                                     |
| **LangChain Tools**                 | Custom functions integrated with the AI agent for logs and issue resolution |

---

## Custom Tools

### `log_extraction_tool`

Extracts log data from file paths provided in incident descriptions.

### `Kerberos_Issue_func`

Outlines automated steps to force-start jobs in Autosys using a JSON payload for ServiceNow requests.

---

## Agent Logic

The AI orchestrates multiple sub-agents:

* **Classifier Agent** – Determines the type of incident
* **SQL Agent** – Queries relevant data from SQLite
* **Incident Agent** – Performs reasoning and generates reports
* **Subtools** – Handle logs, notifications, and remediation steps

Each agent operates using LangChain’s **ReAct** (Reason + Act) architecture with intermediate steps shown in the Streamlit interface.

---

## Future Enhancements

* Integration with real ITSM systems (ServiceNow, Jira)
* Multi-database support (PostgreSQL, Snowflake, etc.)
* Persistent memory and vector knowledge bases
* Role-based access and authentication for enterprise deployment
* Enhanced reporting with PDF exports

---

## Author

Developed by **Kingston**
For enterprise AI-driven incident management and analysis systems.

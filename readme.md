# SOCRadar Incident Connector for Microsoft Sentinel (Logic App)

This Azure Logic App solution automatically integrates security events from the **SOCRadar** threat intelligence platform, creating native **Incidents** directly inside your Microsoft Sentinel workspace.

## 🚀 Quick Deployment

Click the **Deploy to Azure** button below to create the Logic App and all necessary connections in a single step using the Azure Portal.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://github.com/Radargoger/socradar-sentinel-alarm-connector-api/edit/main/readme.md)

## ⚙️ Solution Architecture & Functionality

This Consumption Logic App runs on a recurrent schedule to pull new SOCRadar alerts and inject them as actionable security incidents into Microsoft Sentinel.

| Component | Function | Configuration |
| :--- | :--- | :--- |
| **Recurrence Trigger** | Starts the workflow. | Runs every **3 minutes** (Configurable) |
| **HTTP Action** | Fetches new alerts from the SOCRadar API. | Uses `start_date` parameter to only pull incidents from the last 3 minutes. |
| **Parse JSON** | Structures the incoming SOCRadar response data. | Uses the provided schema for mapping. |
| **For Each / Create Incident** | Iterates through each alert and creates a Sentinel Incident using the Managed Connector. | Maps SOCRadar fields to Sentinel Incident fields (Title, Severity, Description). |

### Field Mapping

The Logic App maps critical SOCRadar data to Sentinel Incident properties:

| SOCRadar Field | Microsoft Sentinel Incident Field |
| :--- | :--- |
| `alarm_type_details.alarm_generic_title` + `alarm_id` | **Title** (e.g., "Data Leak Alarm 12345") |
| `alarm_risk_level` | **Severity** (`High`, `Medium`, etc.) |
| `alarm_text` | **Description** |
| Hardcoded "New" | **Status** |

## 📝 Prerequisites

Ensure you have the following information and resources ready before deployment:

1.  **Azure Subscription** with an active **Microsoft Sentinel** configuration.
2.  **SOCRadar API Key** (Required for the HTTP Action).
3.  **Microsoft Sentinel Workspace Path:** The full resource path for your Sentinel workspace where incidents will be created.

### Workspace Path Requirements (Critical)

The `Create_incident` action in the Logic App is hardcoded to a specific Sentinel workspace path. **You must update this during or after deployment** to match your environment.

**Example Path Structure:**

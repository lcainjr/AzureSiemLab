
# Azure Sentinel SIEM Lab Walkthrough

## 🧠 Objective
Build and simulate a basic SIEM environment in Azure using Microsoft Sentinel to ingest logs, create detection rules, and triage a sample incident.

## 🛠️ Tools Used
- Microsoft Sentinel (Defender for SIEM)
- Azure Virtual Machine (Windows Server/11)
- Log Analytics Workspace
- Azure Monitor Agent (AMA)
- Data Collection Rules (DCR)
- Kusto Query Language (KQL)

## 🔧 Updated Setup Instructions

### 1. Environment Setup
- Create a **Resource Group** in Azure.
- Deploy a **Log Analytics Workspace** in the same region.
- Enable **Microsoft Sentinel** on that workspace.

### 2. Virtual Machine Configuration
- Deploy a **Windows Virtual Machine**.
- Install **Azure Monitor Agent** (AMA) via Extensions.
- Ensure the VM is connected to your Log Analytics workspace.

### 3. Configure Log Ingestion via DCR
- Go to **Azure Monitor > Data Collection Rules (DCRs)**.
- Create a DCR with the following settings:
  - **Windows Event Logs** → Add Log: `Security`
  - Select log levels (e.g., Information, Warning, Error)
  - Assign the VM and link it to the Log Analytics Workspace

### 4. Verify Logs Are Flowing
- Go to **Log Analytics Workspace > Logs**
- In the **Query hub**, switch to **DQL mode**
- Run the following queries:
  ```kusto
  SecurityEvent | limit 10
  ```
  If no data appears, try:
  ```kusto
  union withsource=TableName *
  | summarize Count = count() by TableName
  | order by Count desc
  ```

### 5. Create a Detection Rule in Sentinel
- In **Sentinel**, go to **Content hub**
- Search for **Windows Security Events** and **Install**
- After installation, go to the solution > **Manage** > **Data connectors**
- Open connector page and verify it's connected
- In Sentinel, go to **Analytics > Create Rule**
  - Use a template or custom rule (e.g., “Multiple Failed RDP Attempts”)
  - Save and enable the rule

### 6. Simulate and Test Alerts
- On your VM, simulate multiple failed login attempts
- Go to **Sentinel > Incidents** to verify if the rule triggered

## 🔍 Sample Queries

**Top Security Events**
```kusto
SecurityEvent
| summarize count() by EventID
| top 10 by count_
```

**Heartbeat Check**
```kusto
Heartbeat
| top 10 by TimeGenerated
```

## 📘 What I Learned
- How to configure Azure Sentinel with a Log Analytics workspace
- How to use DCRs and AMA to ingest Windows Security logs
- How to run KQL queries to verify data and hunt incidents
- How to simulate detection rules and alerts in Sentinel

## 🔗 Resources
- [Microsoft Sentinel Docs](https://learn.microsoft.com/en-us/azure/sentinel/)
- [KQL Overview](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/)
- [Microsoft Learn Sentinel Labs](https://learn.microsoft.com/en-us/training/modules/intro-azure-sentinel/)

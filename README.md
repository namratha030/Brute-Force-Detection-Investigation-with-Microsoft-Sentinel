# 🔐 Brute Force Detection & Investigation with Microsoft Sentinel

## 🎯 Objective

This project demonstrates how to configure Microsoft Azure Sentinel to detect brute-force login attacks on a virtual machine (Windows or Linux). It walks through log collection, analytics rule creation using Kusto Query Language (KQL), incident generation, and automated response using Logic Apps.

---

## 🛠 Tools & Services Used

- Microsoft Azure Portal
- Microsoft Sentinel (Defender for SIEM)
- Log Analytics Workspace
- Azure Virtual Machine (Windows/Linux)
- Azure Security Center (Optional)
- Kusto Query Language (KQL)

---

## ✅ Step-by-Step Implementation

### 1️⃣ Access Microsoft Sentinel
- Sign in to the [Azure Portal](https://portal.azure.com/)
- Search for and open **Microsoft Sentinel**
- Click **+ Add** to start configuring Sentinel

### 2️⃣ Create/Select Log Analytics Workspace (LAWS)
- If not already created:
  - Go to **Log Analytics Workspace** → Click **+ Create**
  - Assign a name, resource group, and region
- Attach Sentinel to the Workspace:
  - From Microsoft Sentinel → Click **+ Add** and select the workspace

### 3️⃣ Configure Sentinel Environment
- Explore Sentinel Dashboard:
  - **Data Connectors** – Integrate services like VM, Office 365, etc.
  - **Analytics** – Define rules to detect threats
  - **Workbooks** – Visualize data
  - **Incidents** – View generated alerts

---

## 🔍 Next Phase: Brute Force Attack Detection

### 4️⃣ Connect Virtual Machine to LAWS
- Install **Log Analytics Agent** on the VM
- Register the agent with the Workspace using Workspace ID and Key
- Enable **Security and Audit logs** (especially for Windows VM)

### 5️⃣ Send Logs to Sentinel
- Ensure **SecurityEvent** (Windows) or **Syslog/Auth.log** (Linux) is sent to Sentinel
- Validate logs via:
  ```kql
  SecurityEvent | take 10



  https://github.com/user-attachments/assets/fbfc03d8-a083-4cd4-b8ba-383f99a12532
  

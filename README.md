# 🔐 Brute Force Detection & Investigation with Microsoft Sentinel

## 🔖 Project Title
**Brute Force Detection & Investigation with Microsoft Sentinel**

---

## 🎯 Objective

To detect brute force login attacks using Microsoft Azure Sentinel by:

- Connecting virtual machine logs to Sentinel
- Creating analytics rules using Kusto Query Language (KQL)
- Generating and managing incidents
- Enabling automated threat response

---

## 🛠 Tools Used

- **Microsoft Azure Portal**
- **Microsoft Sentinel (Defender for SIEM)**
- **Log Analytics Workspace (LAWS)**
- **Azure Virtual Machine** (Windows/Linux)
- **Kusto Query Language (KQL)**

---

## 📹 Steps Demonstrated in the Video

### ✅ 1. Accessing Microsoft Sentinel
- Opened Azure Portal in a browser.
- Navigated to **Microsoft Sentinel** service.
- Displayed dashboard and overview panel.

### ✅ 2. Log Analytics Workspace (LAWS) Selection
- Showed the process of:
  - Selecting an existing or creating a new Log Analytics Workspace.
  - This workspace is used to store logs from connected VMs and other resources.

### ✅ 3. Initial Configuration Overview
- Demonstrated:
  - Attaching Sentinel to the LAWS.
  - Exploring Sentinel components:
    - **Data Connectors**
    - **Analytics Rules**
    - **Incidents**

---

## 🛡 Core Concept Demonstrated

The video covers **Phase 1** of the brute force detection project — configuring Microsoft Sentinel and Log Analytics Workspace.  
Without this initial setup, logs cannot be collected, and analytics cannot be performed.

---

## 🔍 Next Steps in the Full Project (Beyond the Video)

### 🔹 1. Connect Azure VM to the Workspace
- Install **Log Analytics Agent** on the VM.
- Connect the VM using the Workspace ID and Key.

### 🔹 2. Send Logs to Sentinel
- Ensure security logs are forwarded:
  - **Windows:** Event ID `4625` (failed login)
  - **Linux:** `Syslog` entries containing `"Failed password"`

### 🔹 3. Create Analytics Rule with KQL
Example query for Windows:
```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count()
    by Account, IPAddress = RemoteHost, bin(TimeGenerated, 5m)
| where FailedAttempts >= 5




# 🚨 What is a Brute Force Attack?

A **Brute Force Attack** is a method where attackers attempt to gain unauthorized access by systematically trying many combinations of usernames and passwords. These attacks are often detected by monitoring for **multiple failed login attempts** within a short period.

---

# ⚙️ How Microsoft Sentinel Helps

Microsoft Sentinel, a cloud-native SIEM solution, enables:

- **Log Collection** from Azure Virtual Machines (Windows/Linux)
- **Real-time Analysis** using Kusto Query Language (KQL)
- **Threat Detection** through Analytics Rules
- **Alerting & Incident Management**
- **Data Visualization** using custom Workbooks

---

# 🛠️ Steps to Detect Brute Force Attacks

## ✅ Step 1: Collect Security Logs

1. **Deploy a VM** (Windows or Linux) in Azure.
2. **Connect the VM** to a **Log Analytics Workspace (LAWS)**.
3. Ensure relevant security logs are collected:
   - **Windows:** `SecurityEvent` with `Event ID 4625` (Failed Logon)
   - **Linux:** `Syslog` entries containing `"Failed password"`

---

## ✅ Step 2: Create Analytics Rule in Sentinel

### 📌 KQL Query for Windows

```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() 
    by Account, IPAddress = RemoteHost, bin(TimeGenerated, 5m)
| where FailedAttempts >= 5




## Step 1: Accessing Microsoft Sentinel

This step demonstrates how to access Microsoft Sentinel in the Azure Portal and set up the initial environment for brute force attack detection.

- Open the [Azure Portal](https://portal.azure.com/)
- Navigate to **Microsoft Sentinel** service
- Explore the Sentinel dashboard and overview panel
- Understand the workspace selection and initial configuration options such as Data Connectors, Analytics Rules, and Incidents

🎥 Watch the walkthrough video here:  
[Accessing Microsoft Sentinel - Step 1](https://github.com/user-attachments/assets/e765d1cd-e395-4427-89b6-b34899715d7b)

# 🔐 Brute Force Detection & Investigation with Microsoft Sentinel

## 📘 Project Overview

A **Brute Force Attack** occurs when an attacker systematically attempts multiple username and password combinations to gain unauthorized access to a system. These are often characterized by a high volume of failed login attempts in a short time frame.

This project demonstrates how to configure **Microsoft Sentinel** to detect and investigate brute-force login attempts on an Azure Virtual Machine (Windows or Linux). It includes log collection, analytic rule creation using **Kusto Query Language (KQL)**, incident generation, and response automation using **Logic Apps**.

---

## 🎯 Objective

To build a detection system using Microsoft Sentinel that:

- Collects and analyzes VM login data.
- Identifies brute-force attempts based on failed logins.
- Automatically generates incidents.
- Visualizes attack trends using dashboards.
- Enables automated response actions.

---

## 🛠 Tools & Services Used

- **Microsoft Azure Portal**
- **Microsoft Sentinel (SIEM)**
- **Log Analytics Workspace (LAWS)**
- **Azure Virtual Machine** (Windows or Linux)
- **Kusto Query Language (KQL)**
- **Azure Logic Apps** *(optional for automation)*
- **Azure Security Center** *(optional for posture management)*

---

## ✅ Step-by-Step Implementation

### 1️⃣ Access Microsoft Sentinel

- Log into [Azure Portal](https://portal.azure.com/)
- Search for and open **Microsoft Sentinel**
- Click **+ Add** to attach Sentinel to an existing or new **Log Analytics Workspace**
- https://github.com/user-attachments/assets/fbfc03d8-a083-4cd4-b8ba-383f99a12532

---

### 2️⃣ Create or Select Log Analytics Workspace

- Go to **Log Analytics Workspaces**
- Click **+ Create** to set up a workspace (name, region, and resource group)
- After creation, attach it to Sentinel

---

### 3️⃣ Connect Azure VM to the Workspace

- Deploy a **Windows or Linux VM**
- Install the **Log Analytics Agent** on the VM
- Use Workspace ID and Key to connect the VM to the LAWS
- Ensure Security logs are being collected:
  - **Windows**: SecurityEvent (Event ID 4625 for failed logins)
  - **Linux**: Syslog logs with "Failed password" entries

---

## 🔍 Detecting Brute Force Attacks

### ✅ Step 4: Create Analytics Rule in Sentinel

#### 🔹 Sample KQL for **Windows VM**:

```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() 
    by Account, IPAddress = RemoteHost, bin(TimeGenerated, 5m)
| where FailedAttempts >= 5

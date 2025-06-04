# Brute Force Detection & Investigation with Microsoft Sentinel

---

## Project Overview

This project demonstrates how to detect and investigate brute force attacks—where attackers repeatedly try multiple username/password combinations to gain unauthorized access—using **Microsoft Azure Sentinel**. By connecting Azure Virtual Machine logs to Sentinel, creating custom analytics rules with Kusto Query Language (KQL), and visualizing attack patterns, security teams can monitor and respond effectively to login-based threats in real time.

---

## What is a Brute Force Attack?

A brute force attack is a cyberattack method where an attacker systematically attempts numerous combinations of usernames and passwords to gain unauthorized access. These attacks typically manifest as a high number of failed login attempts within a short timeframe, which can be detected via security logs.

---

## How Microsoft Sentinel Helps

Microsoft Sentinel is a cloud-native Security Information and Event Management (SIEM) platform that:

- Collects security logs from Azure Virtual Machines (Windows/Linux)
- Analyzes logs in real time using custom Analytics Rules built with Kusto Query Language (KQL)
- Detects suspicious behavior such as brute force login attempts
- Generates alerts and incidents for security teams to investigate
- Provides Workbooks to visualize attack patterns and metrics on customizable dashboards

---

## Tools Used

- Microsoft Azure Portal  
- Microsoft Sentinel (Azure Defender for SIEM)  
- Log Analytics Workspace (LAWS)  
- Azure Virtual Machines (Windows/Linux)  
- Kusto Query Language (KQL)

---

## Project Objectives

- Connect virtual machines to Azure Sentinel through Log Analytics Workspace  
- Collect security logs related to login attempts  
- Create KQL-based custom Analytics Rules to detect brute force attack patterns  
- Trigger alerts and manage incidents based on detection  
- Visualize login failures and attacker details on Sentinel Workbooks  
- Lay groundwork for automating responses such as blocking suspicious IPs

---

## Step-by-Step Implementation

### Step 1: Access Microsoft Sentinel

- Open the [Azure Portal](https://portal.azure.com) in your browser  
- Navigate to **Microsoft Sentinel** service  
- View the Sentinel dashboard and overview panel  

### Step 2: Select or Create Log Analytics Workspace

- Select an existing Log Analytics Workspace or create a new one  
- This workspace stores logs from Azure resources including VMs  

### Step 3: Initial Configuration

- Attach Microsoft Sentinel to the Log Analytics Workspace  
- Explore key components like Data Connectors, Analytics Rules, and Incidents  

> This foundational setup enables log ingestion and threat detection capabilities within Sentinel.

### Step 4: Connect Azure VM to Log Analytics Workspace

- Deploy a VM (Windows or Linux) in Azure  
- Connect the VM to the Log Analytics Workspace for log forwarding  

### Step 5: Collect Security Logs

- For **Windows VMs**: Monitor Security Events with Event ID **4625** (failed login)  
- For **Linux VMs**: Monitor Syslog entries with messages containing **"Failed password"**  

### Step 6: Create Custom Analytics Rule to Detect Brute Force Attacks

- Go to **Microsoft Sentinel > Analytics > + Create**  
- Use the following sample KQL queries depending on your VM OS:  

#### Sample KQL Query for Windows

#### Sample KQL Query for Windows

#### Sample KQL Query for Windows

```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() by Account, IPAddress
| where FailedAttempts >= 5
```
## Step-by-Step Implementation
### Steps to Create a Resource Group:1
Log in to the Azure Portal
🔗 https://portal.azure.com

In the search bar at the top, type:
Resource groups → Click on the Resource groups service.

Click + Create at the top left of the page.

Fill in the required details:

Subscription: Select your Azure subscription (e.g., Free Trial).

Resource group name: Enter a name, e.g., BruteForce-RG

Region: Choose the region where your resources will reside (e.g., Central India, East US).

Click Review + Create.

After validation passes, click Create
🎥 [Watch the Video Walkthrough](https://github.com/user-attachments/assets/e765d1cd-e395-4427-89b6-b34899715d7b)





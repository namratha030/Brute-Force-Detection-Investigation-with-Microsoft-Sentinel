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
### 1) Steps to Create a Resource Group:
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
🎥 [Watch the Video Walkthrough](https://github.com/user-attachments/assets/1dae53e5-a30c-4bd3-8c78-de5a613ec69c)

### 2)Steps to Create a Log Analytics Workspace:
Log in to the Azure Portal
🔗 https://portal.azure.com

In the search bar, type:
Log Analytics workspaces → Click the service.

Click + Create (top left).Click Review + Create.

After validation, click Create.

✅ After Creation:
You now have a Log Analytics Workspace named BruteForceLogs under your resource group BruteForce-RG.

This workspace will:

Receive logs from your VM

Let Sentinel run KQL queries for detection

Be linked in the next step to Microsoft Sentinel







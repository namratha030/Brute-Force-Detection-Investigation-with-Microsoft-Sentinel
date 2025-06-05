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

### 3)Steps to Enable Microsoft Sentinel:
Log in to the Azure Portal
🔗 https://portal.azure.com

In the search bar, type:
Microsoft Sentinel → Click the service (may also appear as Microsoft Defender for SIEM in new versions).

Click + Create / + Add at the top.

Select your Log Analytics Workspace:

Subscription: Select the one you're using (e.g., Free Trial)

Workspace: Choose the one created earlier → BruteForceLogs

Click Add Microsoft Sentinel.

### 4)Create a VM to Access via RDP Protocol
Prerequisites:
You have an Azure account.

You have created a Resource Group.

You want to create a Windows VM (to use RDP).

Step-by-Step Guide:
1. Log in to Azure Portal
Go to https://portal.azure.com

Sign in with your credentials.

2. Navigate to 'Create a Virtual Machine'
On the Azure portal homepage, click Create a resource (top-left corner).

Search for Virtual Machine and select Virtual Machine.

Click Create > Azure virtual machine.

3. Basics Tab
Subscription: Select your subscription.

Resource group: Select the resource group you created earlier or create a new one.

Virtual machine name: Give a name for your VM (e.g., MyWinVM).

Region: Choose the Azure region closest to you.

Availability options: Choose as per your requirement (default: no infrastructure redundancy required).

Image: Select Windows Server 2019 Datacenter (or any Windows OS).

Size: Choose the VM size based on your needs (e.g., Standard B2s).

Username: Enter an admin username.

Password: Enter a strong password (this will be used to log in via RDP).

Inbound port rules: Select Allow selected ports and check RDP (3389).

4. Disks Tab
Select the OS disk type (e.g., Standard SSD or Premium SSD).

You can keep other settings default or customize based on performance needs.

5. Networking Tab
Virtual network: Default or create a new VNet.

Subnet: Default or custom.

Public IP: Ensure a public IP address is assigned to allow RDP access from the internet.

NIC network security group: Choose Basic and allow RDP (3389) port.

Confirm Accelerated networking is off (default).

6. Management, Advanced, Tags Tabs
Leave default or configure based on your needs (usually defaults are fine for basic VM).

7. Review + Create
Azure will validate the settings.

Once validation passes, click Create.

Wait for deployment to complete (few minutes).

8. Access VM via RDP
Go to your Virtual Machines in Azure Portal.

Select your created VM.

Copy the public IP address from the Overview page.

On your local machine, open Remote Desktop Connection (Windows built-in tool).

Enter the public IP address.

Connect using the username and password you set during VM creation.

You will now access your VM desktop via RDP.

### 5)Step 5: View and Analyze Logs in Azure Sentinel
1. Navigate to Azure Sentinel
In the Azure Portal, search for and select Microsoft Sentinel.

Open the Sentinel workspace you set up earlier (linked to your Log Analytics workspace).

2. Go to Logs (Log Analytics)
In your Sentinel workspace menu, click on Logs.

This opens the Log Analytics query editor where you can run Kusto Query Language (KQL) queries against your collected data.

3. Check Incoming Logs
Run simple queries to verify logs from your VM are flowing into Sentinel.

Example:

kql
Copy
Edit
// Check Security Event logs from your VM
SecurityEvent
| where TimeGenerated > ago(1h)
| sort by TimeGenerated desc
This will show recent Windows Security Events (like login attempts).

4. Detect Brute Force or Failed Login Attempts
Use this KQL query to find multiple failed RDP login attempts, which may indicate brute force activity:

kql
Copy
Edit
SecurityEvent
| where TimeGenerated > ago(1d)
| where EventID == 4625  // Failed login event ID on Windows
| summarize FailedAttempts = count() by Account, IPAddress = tostring(parse_json(EventData).IpAddress), bin(TimeGenerated, 1h)
| where FailedAttempts > 5
| order by FailedAttempts desc
This query summarizes failed login attempts by account and IP address in hourly bins, showing only those with more than 5 failed attempts.

### 6) Investigate Incidents (Brute Force Attack Detection in Azure Sentinel)
Once an Analytics Rule (Step 5) triggers — for example, due to multiple failed RDP login attempts (Event ID 4625) — Microsoft Sentinel automatically creates an incident. Here's how you can investigate it:

🔍 1. Go to the Incidents Tab
In the Microsoft Sentinel workspace, click on Incidents in the left-hand menu.

This shows a list of all security incidents generated from analytics rules.

🛠 2. Open the Specific Incident
Click on the incident that was created from your brute force detection rule.

This opens a detailed incident view with:

Alert details

Entities involved (e.g., accounts, IP addresses)

Timestamps

Severity level (e.g., Low, Medium, High)

🧩 3. Review the Alert Details
Under the "Alert Details" tab:

See the query that triggered the alert.

View how many failed login attempts occurred.

Check which IP address or account tried to brute force the system.

🧠 4. Analyze Entities (Accounts, IPs, Hosts)
Sentinel automatically extracts entities such as:

Account Name (targeted account)

Source IP Address (attacker’s IP)

Host Name (target VM

### 7) View Logs in Pie Chart and Bar Graph (Using Workbooks in Sentinel)
Purpose: Visualizing logs (like failed logins, IPs, users) helps SOC teams quickly identify patterns and anomalies in brute force attacks.

🎯 Tools Used:
Microsoft Sentinel

Workbooks (Sentinel dashboard for charts/graphs)

KQL Queries for data representation

🪜 Step-by-Step Process:
🧭 1. Open Workbooks in Sentinel
Go to Microsoft Sentinel from Azure Portal.

Select your Sentinel workspace.

In the left menu, click on Workbooks.

📋 2. Use an Existing Workbook or Create New One
You can choose from built-in templates like:

“Security Insights”

“Sign-in logs”

“Security Events”

OR click + New to create your custom Workbook.

💻 3. Add Visualizations
In the new workbook:

Click "Add query"

Choose your data source: usually SecurityEvent

Paste your KQL query (see examples below)

Choose the chart type: Pie, Bar, Time Chart, etc.

📊 Example 1: Pie Chart – Failed Logins by IP Address
kql
Copy
Edit
SecurityEvent
| where TimeGenerated > ago(1d)
| where EventID == 4625
| summarize FailedAttempts = count() by IPAddress = tostring(parse_json(EventData).IpAddress)
| sort by FailedAttempts desc
Select Pie chart as the visualization type.

This shows which IPs are responsible for most failed logins (brute force sources).

📈 Example 2: Bar Graph – Failed Logins Over Time
kql
Copy
Edit
SecurityEvent
| where TimeGenerated > ago(1d)
| where EventID == 4625
| summarize FailedAttempts = count() by bin(TimeGenerated, 1h)
| render barchart
Select Bar chart or Time chart

This shows brute force attack intensity per hour.

🧑‍💻 Example 3: Bar Graph – Failed Logins by Username
kql
Copy
Edit
SecurityEvent
| where TimeGenerated > ago(1d)
| where EventID == 4625
| summarize FailedAttempts = count() by TargetUserName
| sort by FailedAttempts desc
Helps detect which user accounts are being attacked the most.

🧷 4. Customize the Workbook
Add text blocks, filters, and drop-downs (like time range or IP address).

Give your workbook a title (e.g., “Brute Force Attack Dashboard”).

Save it for reuse and team access

## 📹 Step 1: Video Walkthrough

## 📹 Video Walkthroughs

### ✅ Step 1
🎥 [Watch Step 1 Video Walkthrough](https://drive.google.com/file/d/1_xhwERi8975dD20l1oC9Z83RlSrIT6Dl/view?usp=drive_link)

### ✅ Step 2
🎥 [Watch Step 2 Video Walkthrough](https://drive.google.com/file/d/1sl6F6AM_4VpFwP3d7MvKdCFT2wA-Pg6l/view?usp=drive_link)

### ✅ Step 3
🎥 [Watch Step 3 Video Walkthrough](https://drive.google.com/file/d/1zH7Gw3qEkvmeKEv77FqmCVxlhSqpx1lQ/view?usp=drive_link)

### ✅ Step 4 – Part 1
🎥 [Watch Step 4 (Part 1) Video Walkthrough](https://drive.google.com/file/d/1LkotJgb9m--_m9MIM4Bv3LbwC61XT6Il/view?usp=drive_link)

### ✅ Step 4 – Part 2
🎥 [Watch Step 4 (Part 2) Video Walkthrough](https://drive.google.com/file/d/1iCPpAdnjiBGnwxMiPcXdQOHvivvjyh_0/view?usp=drive_link)

### ✅ Step 5
🎥 [Watch Step 5 Video Walkthrough](https://drive.google.com/file/d/1RAOWMPcUIeAJC1El2A_o_-W_eedkaert/view?usp=drive_link)

### ✅ Step 6
🎥 [Watch Step 6 Video Walkthrough](https://drive.google.com/file/d/1T0o7nWfUqULf1QcsMNpFaA_OgsL-ZPCF/view?usp=drive_link)

### ✅ Step 7
🎥 [Watch Step 7 Video Walkthrough](https://drive.google.com/file/d/1SEQ8GWCOcvgM4Y81FPhewhHecsHd8lxe/view?usp=drive_link)











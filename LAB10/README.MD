# VMware to Azure VM Migration: Detailed Report

# Members Of The Group

### Author: Jimish Gajjar

### Student Number: 041174142

### Email: gajj0008@algonquinlive.com

### Author: Yash Rajput

### Student Number: 041166479

### Email: rajp0026@algonquinlive.com

## Table of Contents

1. [Introduction](#introduction)
2. [Assessment Report](#assessment-report)
   - [Environment Analysis](#environment-analysis)
   - [Dependency Mapping](#dependency-mapping)
   - [Target Configuration Recommendation](#target-configuration-recommendation)
3. [Migration Outcome](#migration-outcome)
   - [Step 1: Configure Replication Appliance](#step-1-configure-replication-appliance)
   - [Step 2: Install and Configure Mobility Agent](#step-2-install-and-configure-mobility-agent)
   - [Step 3: Initiate Replication](#step-3-initiate-replication)
   - [Step 4: Migrate VM to Azure](#step-4-migrate-vm-to-azure)
   - [Step 5: Post-Migration Validation](#step-5-post-migration-validation)
4. [Validation Results](#validation-results)
5. [Reflection](#reflection)

---

## Introduction

This document provides a highly detailed walkthrough of migrating a VMware virtual machine (VM) to Microsoft Azure. The migration was conducted using Azure Migrate services and divided into five main phases: **assessment**, **configuration**, **replication**, **migration**, and **validation**.

---

## Assessment Report

### Environment Analysis

#### VMware VM Details

1. **VM Name:** `Vm-1`
2. **Operating System:** Windows Server 2019
3. **Configuration:**
   - CPUs: 2 vCPUs
   - RAM: 2 GB
   - Disk: 60 GB
4. **Applications Hosted:**
   - `IIS Server - Web Server`
5. **Network Configuration:**
6. **Screenshot:** VMware VM overview.

   ![VM Overview](./screenshots/1.%20Create%20a%20New%20VM%20in%20VMware%20Workstation%20.png)

#### VM Resource Utilization

- Average CPU Utilization: `2 Coree`
- Average Memory Utilization: `2GB`
- Disk Usage: `60`

---

### Dependency Mapping

1. Identified dependencies:
   - **Applications:** `<Application Names>`
   - **Services:** `<Service Names>`
   - **Databases:** `<Database Names>`
2. Actions Taken:
   - Documented all dependency configurations and settings.
   - Confirmed availability of compatible target services in Azure.

---

### Target Configuration Recommendation

1. **Target Azure VM Configuration:**
   - VM Size: `Standard D4s_v3`
   - Region: `Central Canada`
   - Storage: `Managed Disks`
   - Network: `Azure-VNet`
2. **Target Resource Group:**
   - Created new resource group: `Lab-VM-Migration-Done`.
3. **Subnet:**
   - Configured default subnet in the target VNet.

---

## Migration Outcome

### Step 1: Configure Replication Appliance

1. **Download the Azure Migrate Replication Appliance Setup:**

   - Navigate to **Azure Migrate > Servers > Migration tools**.
   - Select **Replication Appliance** and download the setup file.

2. **Deploy the Appliance on VMware:**

   - Upload the file to the VMware server.
   - Deploy it as a virtual machine using the VMware client.

3. **Assign Static IP to the Appliance:**

   - Access the appliance console.
   - Configure:
     - **IP:** `192.168.88.134`
     - **Subnet Mask:** `255.255.255.0`
     - **Gateway:** `192.168.88.2`
     - **DNS:** `192.168.88.2`

4. **Register the Appliance with Azure:**

   - Copy the registration key from Azure Migrate.
   - Enter it during the appliance setup wizard.

   - **Screenshot:** Replication Appliance Configuration.
     ![Replication Appliance Configuration](./screenshots/17.%20Download%20the%20Azure%20Migrate%20Agent%20In%20Azure%20Portal%201.png)

---

### Step 2: Install and Configure Mobility Agent

1. **Download the Mobility Agent:**

   - Connect to the vm1.
   - Locate the agent setup in the directory:
     `C:\ProgramData\jimish\home\svsystems\bin\Installer`

2. **Copy the Agent to the VMware VM:**

   - Use an file transfer tools to move the installer to the VM.

3. **Install the Agent:**

   - Run the installer and proceed with the default setup options.

4. **Configure the Agent:**

   - Enter:
     - Replication Appliance IP
   - Restart the VM after configuration.

   - **Screenshot:** Mobility Agent Installation.
     ![Mobility Agent](./screenshots/47.%20Installing%20mobility%20serever%20in%20vm%201.png)

---

### Step 3: Initiate Replication

1. **Verify Source VM in Azure Portal:**

   - Confirm the VMware VM is listed under **Replicating Machines**.

2. **Configure Replication Settings:**

   - Target Resource Group: `Lab-VM-Migration-Done`
   - Target VNet and Subnet: `Azure-VNet` > `Default`

3. **Start Replication:**

   - Monitor replication progress under the Azure Migrate portal.

   - **Screenshot:** Replication in Progress.
     ![Replication Status](./screenshots/56.%20Server%20replicating%20done%20with%20site%20recover%20jobs.png)

---

### Step 4: Migrate VM to Azure

1. **Complete Replication:**

   - Wait until the replication status shows 100% synchronized.

2. **Initiate Migration:**

   - Select the **Migrate** option.
   - Choose:
     - Shutdown Source VM: `No` (Optional).
     - Migration Target Settings: Auto-configured.

3. **Monitor Migration Progress:**

   - Use the **Notifications** tab in the Azure portal.

   - **Screenshot:** Migration Successful.
     ![Migration Success](./screenshots/57%20Migration%20Status%20Check.png)

---

### Step 5: Post-Migration Validation

1. **Assign Public IP to the Azure VM:**

   - Navigate to **Networking > NIC > IP Configuration**.
   - Assign a new Public IP.

2. **Configure Security Groups:**

   - Update Network Security Group (NSG) rules to allow vm and application ports.

3. **Access the Azure VM:**

   - Use azure connect to the migrated Azure VM.

4. **Validate Data Integrity:**

   - Verify files and folders on the Azure VM.

5. **Test Application Functionality:**
   - Launch the migrated applications.
   - Verify logs and user interactions.

---

## Validation Results

1. **Access Validation:**

   - Successfully accessed the Azure VM using RDP.

2. **Data Integrity Check:**

   - Verified that all files and configurations from the source VM were present.

3. **Application Functionality Validation:**

   - Tested hosted applications and confirmed no errors.

   - **Screenshot:** Application Functionality Test.
     ![Application Validation](./screenshots/58.%20website%20uploded.png)

---

## Reflection

### Challenges Faced

1. **Static IP Configuration:**

   - The replication appliance initially used a dynamic IP, causing registration errors.

2. **Mobility Agent Compatibility:**

   - Older agent versions caused replication health warnings.

3. **Time Constraints:**

   - Data synchronization for large disks was time-consuming.

4. **Migration Issue:**
   - Initially i try to connect vm-1 but it was not able to connect azure and migration is not working then after i try to reinstall mobility agent and then it is working.

### Lessons Learned

1. Ensure all IP configurations are static to prevent connectivity issues.
2. Always use the latest version of the Azure Mobility Agent.
3. Pre-plan target resource group and network configurations for smoother migration.

## Notes: All other screenshots are in screenshots folder so please check screenshots folder under this same github repo for more clarification of the work.

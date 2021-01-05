---
lab:
    title: '3: Managing Azure Role-Based Access Control'
    module: 'Module 3: Implement and Manage Azure Governance'
---

# Lab: Managing Azure Role-Based Access Control
# Student lab manual

## Lab scenario

With Azure Active Directory (Azure AD) becoming integral part of its identity management environment, the Adatum Enterprise Architecture team must also determine the optimal authorization approach. In the context of controlling access to Azure resources, such approach must involve the use of Azure Role-Based Access Control (RBAC). Azure RBAC is an authorization system built on Azure Resource Manager that provides fine-grained access management of Azure resources.

The key concept of Azure RBAC is role assignment. A role assignment consists of three elements: security principal, role definition, and scope. A security principal is an object that represents a user, group, service principal, or managed identity that is requesting access to Azure resources. A role definition is a collection of the operations that the role assignments will grant, such as read, write, or delete. Roles can be generic or resource specific. Azure includes four built-in generic roles (Owner, Contributor, Reader, and User Access Administrator) and a fairly large number of built-in resource-specific roles (such as, for example, Virtual Machine Contributor, which includes permissions to create and manage Azure virtual machines). It is also possible to define custom roles. A scope is the set of resources that the access applies to. A scope can be set at multiple levels: management group, subscription, resource group, or resource. Scopes are structured in a parent-child relationship.

The Adatum Enterprise Architecture team wants to test delegation of Azure management by using custom Role-Based Access Control roles. To start its evaluation, the team intends to create a custom role that provides restricted access to Azure virtual machines. 
  

## Objectives
  
After completing this lab, you will be able to:

-  Define a custom RBAC role 

-  Assign a custom RBAC role


## Lab Environment
  



Estimated Time: 60 minutes


## Lab Files

-  \\\\AZ303\\AllFiles\\Labs\\10\\azuredeploy30310suba.json

-  \\\\AZ303\\AllFiles\\Labs\\10\\azuredeploy30311rga.json

-  \\\\AZ303\\AllFiles\\Labs\\10\\azuredeploy30311rga.parameters.json

-  \\\\AZ303\\AllFiles\\Labs\\10\\roledefinition30311.json


## Instructions

### Exercise 0: Prepare the lab environment

The main tasks for this exercise are as follows:

1. Deploy an Azure VM by using an Azure Resource Manager template

1. Create an Azure Active Directory user


#### Task 1: Deploy an Azure VM by using an Azure Resource Manager template

1.From your lab computer, start a web browser, navigate to the [Azure portal](https://portal.azure.com), and sign in using the azure credentials provided in the Environment Details tab.

1. In the Azure portal, open **Cloud Shell** pane by selecting on the toolbar icon directly to the right of the search textbox.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 

    >**Note**: i) If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab and **click on Show advanced settings**.

 ii) Select existing Resource Group as **az30311a-labRG** and enter unique names for **storage account name** and **File Share** and select **Create storage**.

1. In the toolbar of the Cloud Shell pane, select the **Upload/Download files** icon, in the drop-down menu select **Upload**, and upload the file **\\\\AZ303\\AllFiles\Labs\\10\\azuredeploy30310suba.json** into the Cloud Shell home directory.

1. From the Cloud Shell pane, run the following to create a resource groups (replace the `<Azure region>` placeholder with the name of the Azure region that is available for deployment of Azure VMs in your subscription and which is closest to the location of your lab computer):

      > **Note**: To identify Azure regions where you can provision Azure VMs, refer to [**https://azure.microsoft.com/en-us/regions/offers/**](https://azure.microsoft.com/en-us/regions/offers/)

1. In the toolbar of the Cloud Shell pane, select the **Upload/Download files** icon, in the drop-down menu select **Upload**, and upload the file **C:\AllFiles\AZ-303-Microsoft-Azure-Architect-Technologies-master\AllFiles\Labs\11\azuredeploy30311rga.json** into the Cloud Shell home directory.

1. From the Cloud Shell pane, upload the Azure Resource Manager parameter file **\\\\AZ303\\AllFilesLabs\\10\\azuredeploy30311rga.parameters.json**.

1. From the Cloud Shell pane, run the following to deploy a Azure VM running Windows Server 2019 that you will be using in this lab:


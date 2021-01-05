---
lab:
    title: '2: Implementing and Configuring Azure Storage File and Blob Services'
    module: 'Module 2: Implement Storage Accounts'
---

# Lab: Implementing and Configuring Azure Storage File and Blob Services
# Student lab manual

## Lab scenario
 
Adatum Corporation hosts large amounts of unstructured and semi-structured data in its on-premises storage. Its maintenance becomes increasingly complex and costly. Some of the data is preserved for extensive amount of time to address data retention requirements. The Adatum Enterprise Architecture team is looking for inexpensive alternatives that would support tiered storage, while, at the same time allow for secure access that minimizes the possibility of data exfiltration. While the team is aware of practically unlimited capacity offered by Azure Storage, it is concerned about the usage of account keys, which grant unlimited access to the entire content of the corresponding storage accounts. While keys can be rotated in an orderly manner, such operation needs to be carried out with proper planning. In addition, access keys constitute exclusively an authorization mechanism, which limits the ability to properly audit their usage.

To address these shortcomings, the Architecture team decided to explore the use of shared access signatures. A shared access signature (SAS) provides secure delegated access to resources in a storage account while minimizing the possibility of unintended data exposure. SAS offers granular control over data access, including the ability to limit access to an individual storage object, such as a blob, restricting such access to a custom time window, as well as filtering network access to a designated IP address range. In addition, the Architecture team wants to evaluate the level of integration between Azure Storage and Azure Active Directory, hoping to address its audit requirements. The Architecture team also decided to determine suitability of Azure Files as an alternative to some of its on-premises file shares.

To accomplish these objectives, Adatum Corporation will test a range of authentication and authorization mechanisms for Azure Storage resources, including:

-  Using shared access signatures on the account, container, and object-level

-  Configuring access level for blobs 

-  Implementing Azure Active Directory based authorization

-  Using storage account access keys


## Objectives
  
After completing this lab, you will be able to:

-  Implement authorization of Azure Storage blobs by leveraging shared access signatures

-  Implement authorization of Azure Storage blobs by leveraging Azure Active Directory

-  Implement authorization of Azure Storage file shares by leveraging access keys


## Lab Environment
  



Estimated Time: 90 minutes


## Lab Files

-  \\\\AZ303\\AllFiles\\Labs\\06\\azuredeploy30306suba.json

-  \\\\AZ303\\AllFiles\\Labs\\06\\azuredeploy30302rga.json

-  \\\\AZ303\\AllFiles\\Labs\\06\\azuredeploy30302rga.parameters.json


### Exercise 0: Prepare the lab environment

The main tasks for this exercise are as follows:

 1. Deploy an Azure VM by using an Azure Resource Manager template


#### Task 1: Deploy an Azure VM by using an Azure Resource Manager template

1. From your lab computer, start a web browser, navigate to the [Azure portal](https://portal.azure.com), and sign in by providing credentials of a user account with the Owner role in the subscription you will be using in this lab.

1. In the Azure portal, open **Cloud Shell** pane by selecting on the toolbar icon directly to the right of the search textbox.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 

    If this is the first time you are starting **Cloud Shell** and you will be presented with the **You have no storage mounted** page, select the subscription and click on Show advanced settings. 
Select **Use existing** under Resource Group then select **az30302a-labRG** and enter **shellstorageDeployment-id** for storage account name and Enter **filestorageDeployment-id** then click on **Create Storage**.

1. In the toolbar of the Cloud Shell pane, select the **Upload/Download files** icon, in the drop-down menu select **Upload**, and upload the file **\\\\AZ303\\AllFiles\Labs\\06\\azuredeploy30306suba.json** into the Cloud Shell home directory.

1. From the Cloud Shell pane, run the following to create a resource groups (replace the `<Azure region>` placeholder with the name of the Azure region that is available for deployment of Azure VMs in your subscription and which is closest to the location of your lab computer):


1.   From the Cloud Shell pane, upload the file **C:\AllFiles\AZ-303-Microsoft-Azure-Architect-Technologies-master\AllFiles\Labs\02\azuredeploy30302rga.json** into the Cloud Shell home directory.

1. From the Cloud Shell pane, upload the Azure Resource Manager parameter file **\\\\AZ303\\AllFilesLabs\\06\\azuredeploy30302rga.parameters.json**.

1. From the Cloud Shell pane, run the following to deploy a Azure VM running Windows Server 2019 that you will be using in this lab:


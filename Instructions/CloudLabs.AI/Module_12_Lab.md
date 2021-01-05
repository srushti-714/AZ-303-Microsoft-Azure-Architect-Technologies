---
lab:
    title: '4: Protecting Hyper-V VMs by using Azure Site Recovery'
    module: 'Module 4: Protecting Hyper-V VMs by using Azure Site Recovery'
---

# Lab: Protecting Hyper-V VMs by using Azure Site Recovery
# Student lab manual

## Lab scenario

While Adatum Corporation has, over the years, implemented a number of high availability provisions for their on-premises workloads, its disaster recovery capabilities are still insufficient to address the Recovery Point Objectives (RPOs) and Recovery Time Objectives (RTOs) demanded by its business. Maintaining the existing secondary on-premises site requires an extensive effort and incurs significant costs. The failover and failback procedures are, for the most part, manual and are poorly documented. 

To address these shortcomings, the Adatum Enterprise Architecture team decided to explore capabilities of Azure Site Recovery, with Azure taking on the role of the hoster of the secondary site. Azure Site Recovery automatically and continuously replicates workloads running on physical and virtual machines from the primary to the secondary site. Site Recovery uses storage-based replication mechanism, without intercepting application data. With Azure as the secondary site, data is stored in Azure Storage, with built-in resilience and low cost. The target Azure VMs are hydrated following a failover by using the replicated data. The Recovery Time Objectives (RTO) and Recovery Point objectives are minimized since Site Recovery provides continuous replication for VMware VMs and replication frequency as low as 30 seconds for Hyper-V VMs. In addition, Azure Site Recovery also handles orchestration of failover and failback processes, which, to large extent, can be automated. It is also possible to use Azure Site Recovery for migrations to Azure, although the recommended approach relies on Azure Migrate instead.

The Adatum Enterprise Architecture team wants to evaluate the use of Azure Site Recovery for protecting on-premises Hyper-V virtual machines to Azure VM.

## Objectives
  
After completing this lab, you will be able to:

-  Configure Azure Site Recovery

-  Perform test failover

-  Perform planned failover

-  Perform unplanned failover


## Lab Environment




Estimated Time: 120 minutes


## Lab Files

-  \\\\AZ303\\AllFiles\\Labs\\12\\azuredeploy30312suba.json


### Exercise 0: Prepare the lab environment

The main tasks for this exercise are as follows:

1. Deploy an Azure VM by using an Azure Resource Manager QuickStart template

1. Configure nested virtualization in the Azure VM


#### Task 1: Deploy an Azure VM by using an Azure Resource Manager QuickStart template

1. From your lab computer, start a web browser, navigate to the [Azure portal](https://portal.azure.com), and sign in using the azure credentials provided in the Environment Details tab.

1. In the Azure portal, open **Cloud Shell** pane by selecting on the toolbar icon directly to the right of the search textbox.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 


1. In the toolbar of the Cloud Shell pane, select the **Upload/Download files** icon, in the drop-down menu select **Upload**, and upload the file **\\\\AZ303\\AllFiles\Labs\\12\\azuredeploy30312suba.json** into the Cloud Shell home directory.



1. In the Azure portal, close the **Cloud Shell** pane.

1. From your lab computer, open another browser tab, navigate to the [301-nested-vms-in-virtual-network Azure QuickStart template](https://github.com/Azure/azure-quickstart-templates/tree/master/301-nested-vms-in-virtual-network) and select **Deploy to Azure**. This will automatically redirect the browser to the **Hyper-V Host Virtual Machine with nested VMs** blade in the Azure portal.

1. On the **Hyper-V Host Virtual Machine with nested VMs** blade in the Azure portal, specify the following settings (leave others with their default values):

    | Setting | Value | 
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource group |  **az30307a-labRG-deploymentID** |
    | Host Public IP Address Name | **az30307a-hv-vm-pip** |
    | Virtual Network Name | **az30307a-hv-vnet** |
    | Host Network Interface1Name | **az30307a-hv-vm-nic1** |
    | Host Network Interface2Name | **az30307a-hv-vm-nic2** |
    | Host Virtual Machine Name | **az30307a-hv-vm** |
    | Host Admin Username | **Student** |
    | Host Admin Password | **Pa55w.rd1234** |

1. On the **Hyper-V Host Virtual Machine with nested VMs** blade, select **Review + create** and then select **Create**.

    > **Note**: Wait for the deployment to complete. The deployment might take about 10 minutes.

#### Task 2: Configure nested virtualization in the Azure VM

1. In the Azure portal, search for and select **Virtual machines** and, on the **Virtual machines** blade, select **az30307a-hv-vm**.

1. On the **az30307a-hv-vm** blade, select **Networking**. 

1. On the **az30312a-hv-vm | Networking** blade, select **az30307a-hv-vm-nic1** and then select **Add inbound port rule**.

    >**Note**: Make sure that you modify the settings of **az30307a-hv-vm-nic1**, which has the public IP address assigned to it.

1. On the **Add inbound security rule** blade, specify the following settings (**leave others with their default values**) and select **Add**:

    | Setting | Value | 
    | --- | --- |
    | Destination port ranges | **3389** |
    | Protocol | **Any** |
    | Name | **AllowRDPInBound** |

1. On the **az30307a-hv-vm** blade, select **Overview**. 

1. On the **az30307a-hv-vm** blade, select **Connect**, in the drop-down menu, select **RDP**, and then click **Download RDP File**.

1. Open the RDP file, and when prompted sign in with the following credentials:



1. Within the Remote Desktop session to **az30307a-hv-vm**, in the Server Manager window, click **Local Server**, click the **On** link next to the **IE Enhanced Security Configuration** label, and, in the **IE Enhanced Security Configuration** dialog box, select both **Off** options.

1. Within the Remote Desktop session to **az30307a-hv-vm**, start Internet Explorer, browse to [Windows Server Evaluations](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019), and download the Windows Server 2019 **VHD** file to the **F:\VHDs** folder (you will need to create it first). 

1. Within the Remote Desktop session to **az30307a-hv-vm**, start **Hyper-V Manager**. 

1. In the **Hyper-V Manager** console, select the **az30307a-hv-vm** node, select **New** and, in the cascading menu, select **Virtual Machine**. This will start the **New Virtual Machine Wizard**. 

1. On the **Before You Begin** page of the **New Virtual Machine Wizard**, select **Next >**.

1. On the **Specify Name and Location** page of the **New Virtual Machine Wizard**, specify the following settings and select **Next >**:

    | Setting | Value | 
    | --- | --- |
    | Name | **az30307a-vm1** | 
    | Store the virtual machine in a different location | selected | 
    | Location | "F:\VMs\" |

    >**Note**: Make sure to create the "F:\VMs\" folder.

1. On the **Specify Generation** page of the **New Virtual Machine Wizard**, ensure that the **Generation 1** option is selected and select **Next >**:

1. On the **Assign Memory** page of the **New Virtual Machine Wizard**, set **Startup memory** to **2048** and select **Next >**.

1. On the **Configure Networking** page of the **New Virtual Machine Wizard**, in the **Connection** drop-down list select **NestedSwitch** and select **Next >**.

1. On the **Connect Virtual Hard Disk** page of the **New Virtual Machine Wizard**, select the option **Use an existing virtual hard disk**, set location to the VHD file you downloaded to the **F:\VHDs** folder, and select **Next >**.

1. On the **Summary** page of the **New Virtual Machine Wizard**, select **Finish**.

1. In the **Hyper-V Manager** console, select the newly created virtual machine and select **Start**. 

1. In the **Hyper-V Manager** console, verify that the virtual machine is running and select **Connect**. 

1. In the Virtual Machine Connection window to **az30307a-vm1**, on the **Hi there** page, select **Next**. 

1. In the Virtual Machine Connection window to **az30307a-vm1**, on the **License terms** page, select **Accept**. 

1. In the Virtual Machine Connection window to **az30307a-vm1**, on the **Customize settings** page, set the password of the built-in Administrator account to **Pa55w.rd1234** and select **Finish**. 

1. In the Virtual Machine Connection window to **az30307a-vm1**, sign in by using the newly set password.

1. In the Virtual Machine Connection window to **az30307a-vm1**, start Windows PowerShell and, in the **Administrator: Windows PowerShell** window run the following to set the computer name. 


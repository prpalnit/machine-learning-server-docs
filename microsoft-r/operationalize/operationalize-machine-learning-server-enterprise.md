---

# required metadata
title: "Configure R Server to operationalize analytics (enterprise) - Machine Learning Server | Microsoft Docs"
description: "Configure Operationalization for Microsoft R Server, load balancer, "
keywords: "setup r server for deployment; install r server for deploying"
author: "j-martens"
ms.author: "jmartens"
manager: "jhubbard"
ms.date: "6/19/2017"
ms.topic: "article"
ms.prod: "microsoft-r"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
ms.technology:
  - deployr
  - r-server
#ms.custom: ""
---

# Configure Machine Learning Server to operationalize analytics (enterprise)

**Applies to:  Machine Learning Server 9.2.1**

You can configure Microsoft Learning Server after installation to act as a deployment server and to host analytic web services for operationalization. Machine Learning Server offers two types of configuration for operationalizing analytics and remote execution: [One-box](operationalize-machine-learning-server-one-box.md) and **Enterprise**. This article describes the enterprise configuration.

An enterprise configuration involves multiple [web and compute nodes](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization) that  are configured on multiple machines along with other enterprise features.  These nodes can be scaled independently. Scaling up web nodes enables an active-active configuration that allows you to load balance the incoming API requests.  Additionally, with multiple web nodes, you must use a [SQL Server or PostgreSQL database](../operationalize/configure-remote-database-to-operationalize.md) to share data and web services across web node services.   

For added security, you can [configure SSL](../operationalize/configure-https.md) and authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md) in this configuration.

![Enterprise Configuration](./media/operationalize-machine-learning-server-enterprise/configure-enterprise.png)

## How to upgrade 

To replace an older version, you can uninstall the older distribution before installing the new version (there is no in-place upgrade). 

**Carefully review the following steps.** 

### Upgrade a compute node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall Microsoft R Server 9.0 or 9.1 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](r-server-install-uninstall-upgrade.md). The uninstall process stashes away a copy of your 9.0 or 9.1 configuration files under this directory so you can seamlessly upgrade to Machine Learning Server 9.2 in the next step:
   
   + Windows: `C:\Users\Default\AppData\Local\DeployR\current`

   + Linux: `/etc/deployr/current`

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

   + Windows instructions: [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder (for example, `C:\Program Files\Microsoft SQL Server\140`).

   + Linux instructions: [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. The utility checks to see if any configuration files from past releases are present under the `current` folder mentioned previously.

1. Choose **Configure server** from the menu and then **Configure a compute node** from the submenu. 

1. When the script asks you if you want to upgrade, enter `y`. The node is automatically set up using the configuration you had for R Server 9.0 or 9.1. 
    
You can now **repeat these steps** for each compute node.

<a name="upgradewebnode"></a>

### Upgrade a web node

>[!IMPORTANT]
>Before you begin, back up the appsettings.json file on each node in case of an issue during the upgrade process.

1. Uninstall Microsoft R Server 9.0 or 9.1 using the instructions in the article [Uninstall Microsoft R Server to upgrade to a newer version](r-server-install-uninstall-upgrade.md). The uninstall process stashes away a copy of your 9.0 or 9.1 configuration files under this directory so you can seamlessly upgrade to Machine Learning Server 9.2.1 in the next step:
   
   + Windows: `C:\Users\Default\AppData\Local\DeployR\current`

   + Linux: `/etc/deployr/current`

1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

   + Windows instructions: [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder (for example, `C:\Program Files\Microsoft SQL Server\140`).

   + Linux instructions: [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. The utility checks to see if any configuration files from past releases are present under the `current` folder mentioned previously.

1. Choose **Configure server** from the menu and then **Configure a web node** from the submenu.  

1. When the script asks you if you'd like to upgrade, enter `y`. The node is automatically set up using the configuration you had for R Server 9.0 or 9.1. 

1. From the main menu, choose the option to **Run Diagnostic Tests** to [test the configuration](../operationalize/configure-run-diagnostics.md).

You can now **repeat these steps** for each node.

## How to configure for the enterprise

>[!Important]
>For your convenience, [Azure Management Resource (ARM) templates](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#template-deployment) are available to quickly deploy and configure Machine Learning Server for operationalization in Azure.  
>
>Get one of [these templates on GitHub](https://github.com/Microsoft/microsoft-r/tree/master/rserver-arm-templates). Then, learn how to use it with this [blog post](https://blogs.msdn.microsoft.com/rserver/2017/07/07/set-up-an-auto-scale-environment-to-operationalize-your-r-analytics-with-just-one-click/).

### 1. Configure a database

While the web node configuration sets up a local SQLite database by default, you must use a SQL Server or PostgreSQL database for this configuration for any of the following situations:
+ Have multiple web nodes (so data can be shared across web nodes)
+ Want to achieve higher availability
+ Need a remote database for your web node

To configure that database, [follow these instructions](../operationalize/configure-remote-database-to-operationalize.md).

>[!WARNING] 
>Choose and configure your database now. If you configure a different database later, all data in your current database is lost.

<a name="add-compute-nodes"></a>

### 2. Configure compute nodes

Configure one or more compute nodes as needed. We highly recommend that you configure each node (compute or web) on its own machine for higher availability.

In the Enterprise configuration, side-by-side installations of a web and compute node are not supported.
 
1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

   + Windows instructions: [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder (for example, `C:\Program Files\Microsoft SQL Server\140`).

   + Linux instructions: [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges. 

1. From the main utility menu, choose **Configure server** and then choose **Configure a compute node** from the submenu. 

   >[!NOTE]
   >Bypass this interactive node configuration step using the argument `-silentcomputenodeinstall` when launching the administration utility. Learn about all command line switches for this utility [here](../operationalize/configure-use-admin-utility.md#switch).
    
1. When the configuration is finished, open the port 12805: 

   **Windows**: Add an exception to your firewall to open port 12805. And, for additional security, you can also restrict communication for a private network or domain using a profile.

   **Linux**: If using IPTABLES or equivalent firewall service on Linux, then open the port 12805 using `iptables`  or the equivalent command.

1. From the main utility menu, choose the option **Stop and start services** and restart the compute node so that it can be defined as a service.

You can now **repeat these steps** for each compute node you want to add.


<a name="webnode"></a>

### 3. Configure web nodes

In an enterprise configuration, you can set up one or more web nodes. Note that it is possible to run the web node service from within IIS. 
 
1. Install Machine Learning Server and its dependencies as follows. [Learn about supported platforms for this configuration.](../operationalize/configure-start-for-administrators.md#configure-server-for-operationalization)

   + Windows instructions: [Installation steps](r-server-install-windows.md) | [Offline steps](r-server-install-windows-offline.md)
      
     For _SQL Server Machine Learning Services_, you must also manually install .NET Core 1.1 and add a registry key called `H_KEY_LOCAL_MACHINE\SOFTWARE\R Server\Path` with a value of the parent path to the `R_SERVER` folder (for example, `C:\Program Files\Microsoft SQL Server\140`).

   + Linux instructions: [Installation steps](r-server-install-linux-server.md) | [Offline steps](r-server-install-linux-offline.md)

1. [Launch the administration utility](../operationalize/configure-use-admin-utility.md#launch) with administrator privileges to begin setting up and configuring a web node.

1. On the first web node you configure, declare the IP addresses of every compute node with each web node. These URI declarations are stored in the database. Afterwards, each web node across your configuration can automatically find and communicate with the declared compute nodes. Declare them as follows:
   
   1. From the main utility menu, choose **Manage compute nodes** and then choose **Add URIs** from the submenu.

   1. When prompted, enter the IP address of each compute node you want to configured in the previous step. You can specify a specific URI or  specify port ranges (or IP octets). For multiple compute nodes, separate each URI with a comma. 
      For example: `http://1.1.1.1:12805, http://1.0.1-3.1-2:12805`.

   1. Return the main menu of the utility.

1. Also in the utility, configure the web node AFTER declaring the compute node URIs on at least one web node machine.

   1. From the main menu, choose **Configure server**. Then, choose **Configure a web node** from the submenu. 
  
   1. When prompted, provide a password for the built-in, local operationalization administrator account called 'admin'.  Later, you can configure the server to authenticate against  [Active Directory (LDAP) or Azure Active Directory](../deployr/../operationalize/configure-authentication.md#local).
   
   >[!NOTE]
   >Bypass the interactive steps to install the node and set an admin password using the command-line switches `-silentwebnodeinstall mypassword`. Learn about all command line switches for this utility [here](../operationalize/configure-use-admin-utility.md#switch).

1. In the same utility, test the configuration. From the main utility menu, choose **Run Diagnostic Tests** and choose a [diagnostic test](../operationalize/configure-run-diagnostics.md).

1. In the same utility, restart the web node to define it as a service. From the main utility menu, choose the option **Stop and start services**.

1. Exit the utility.

1. If using the IPTABLES firewall or equivalent service on Linux, then allow remote machines to access the public IP of the web node using the `iptables` command (or the equivalent) to open port 12800.

You can now **repeat these steps** for each web node you want to add.

>[!Important]
>Machine Learning Server uses Kestrel as the web server for its operationalization web nodes. Therefore, if you expose your application to the Internet, we recommend that you review the [guidelines for Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel) regarding reverse proxy setup.

### 4. Setup enterprise-grade security

In production environments, we strongly recommend the following approaches:

1. [Configure SSL/TLS](../operationalize/configure-https.md) and install the necessary certificates.

1. Authenticate against [Active Directory (LDAP) or Azure Active Directory](../operationalize/configure-authentication.md).  

1. For added security, restrict the list of IPs that can access the machine hosting the compute node.


### 5. Provision on the cloud

If you are provisioning on a cloud service, then you must also [create inbound security rule for port 12800 in Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-windows-classic-setup-endpoints/) or open the port through the AWS console. This endpoint allows clients to communicate with the  Machine Learning Server's operationalization server.

### 6. Set up a load balancer

You can set up the load balancer of your choosing. Keep in mind that web nodes are stateless. Therefore, session persistence ("stickiness") is NOT required. 

**Important!** For proper access token signing and verification across your configuration, ensure that the JWT certificate settings are exactly the same for every web node.  These JWT settings are defined on each web node in the configuration file, appsetting.json. [Learn more...](../operationalize/configure-authentication.md#ldap-jwt)

### 7. Post configuration steps

1. [Update service ports](../operationalize/configure-use-admin-utility.md#ports), if needed.

1. [Run diagnostic tests](../operationalize/configure-run-diagnostics.md).

1. [Evaluate](../operationalize/configure-evaluate-capacity.md) the configuration's capacity.

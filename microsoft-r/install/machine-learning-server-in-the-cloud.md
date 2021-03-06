---

# required metadata
title: "Machine Learning Server in the Cloud (Virtual Machines)"
description: "Machine Learning Server in the Cloud: Azure"
keywords: "Machine Learning Server, R Server, azure, virtual machine"
author: "HeidiSteen"
ms.author: "heidist"
manager: "cgronlun"
ms.date: "2/16/2018"
ms.topic: "article"
ms.prod: "mlserver"

# optional metadata
#ROBOTS: ""
#audience: ""
#ms.devlang: ""
#ms.reviewer: ""
#ms.suite: ""
#ms.tgt_pltfrm: ""
#ms.technology: ""
#ms.custom: ""

---

# Machine Learning Server in the Cloud

[Machine Learning Server](../what-is-microsoft-r-server.md), formerly known as Microsoft R Server, is the most broadly deployable enterprise-class analytics platform for R and Python. 

## One-click install using an ARM template

If you have an Azure subscription, you can use an ARM template that performs the following tasks: provisions a virtual machine, installs Machine Learning Server, opens port 12800, and configures the server for operationalization. 

Follow these links to get started:

+ [Machine Learning Server ARM templates on Github](https://github.com/Microsoft/microsoft-r/tree/master/mlserver-arm-templates)
+ [One-click installation of an auto-scale environment to operationalize your R analytics](https://blogs.msdn.microsoft.com/mlserver/2017/07/07/set-up-an-auto-scale-environment-to-operationalize-your-r-analytics-with-just-one-click/)

## Use the portal to create a virtual machine

Alternatively, you can create a virtual machine through the Azure portal. 

+ [R Server 9.1 on Azure HDInsight](machine-learning-server-on-azure-hdinsight.md)

+ [Machine Learning Server as preconfigured Azure virtual machine on Linux or Windows](machine-learning-server-azure-vm-on-linux.md)

+ [SQL Server Machine Learning Server as preconfigured Azure virtual machine on Windows](https://docs.microsoft.com/sql/advanced-analytics/r/provision-the-r-server-only-sql-server-2016-enterprise-vm-on-azure)

+ [Machine Learning Server on the Microsoft Data Science Virtual Machine](r-server-vm-data-science.md)
  
On these virtual machine images, the core engine is configured, but operationalization is not. For instructions, see [Operationalize analytics with Machine Learning Server](../what-is-operationalization.md).
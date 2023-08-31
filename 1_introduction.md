# Azure SQL Database with Pulumi


<details markdown="block" open>
   <summary>Table of Contents</summary>

- [Introduction](https://bitquip.github.io/Azure-SQL/1_introduction)
- [Prerequisites](https://bitquip.github.io/Azure-SQL/2_prerequisites)
- [Setup](https://bitquip.github.io/Azure-SQL/3_setup)
- [Define](https://bitquip.github.io/Azure-SQL/4_define)
- [Deploy](https://bitquip.github.io/Azure-SQL/5_deploy)
- [Cleanup](https://bitquip.github.io/Azure-SQL/6_cleanup)
- [Conclusion](https://bitquip.github.io/Azure-SQL/7_conclusion)

</details>

<br>

![Lord Nibbler](https://static.wikia.nocookie.net/simpsons/images/f/f5/250px-Nibbler.png/)



## Introduction

**This tutorial will walk you through the process of creating an Azure SQL Database using Pulumi.**

Every software engineer at Vizient should strive to be a full-stack engineer. This means that you should be able to create and manage your own infrastructure, as well as your own application code.

The goal of this tutorial is to help you get started with creating and managing your own infrastructure.

To achieve this goal, we will be using Pulumi to create an Azure SQL Database following standard Vizient practices.

We will be using the following tools:

- [Pulumi](https://www.pulumi.com/)
- [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Azure SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [Azure SQL Database Server](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-servers)
- [Azure SQL Database Firewall](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure)
- [Azure SQL Database Firewall Rule](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure#configure-a-server-level-firewall-rule-using-the-azure-portal)
- [Azure SQL Database Connection String](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-connect-query-dotnet-core)

Links have been provided for each tool, but you will not need to read the documentation for each tool to complete this tutorial.

At Vizient, the Cloud Operations team provides Vizient-flavored Pulumi modules that you can use to create and manage your infrastructure.

## Vizient-flavored Pulumi

The Cloud Operations team has created documentation to help you get started with Pulumi.

Vizient-flavored Pulumi modules are modules that have been created by the Cloud Operations team by wrapping the modules with custom functionality to achieve necessary prerequisite resources, best align resource creation with security standards, as well as other desirable outcomes.

The documentation includes how to install Pulumi, how to create a Pulumi project, and how to deploy your Pulumi project.

You can find this documentation on Confluence [here](https://vizientinc.atlassian.net/wiki/spaces/CO/pages/131072512/Pulumi+Getting+Started).

Other pertinent documentation can be found on Confluence by utilizing the search bar at the top.

For this tutorial, we will start fresh and add the necessary resources to create an Azure SQL Database.

We will go through these prerequisite resources somewhat quickly, in order to focus on the Azure SQL Database. (???)

In order to avoid potential conflicts with existing resources being used in live environments among other considerations, we will be creating resources in Vizient's learning environment (ALS - Azure Learning Subscription).


<div style="display: flex; justify-content: space-between; align-items: center;">
    <span style="margin: 10px;"></span>
    <a href="https://bitquip.github.io/Azure-SQL/2_prerequisites" style="margin: 10px; text-decoration: none;">Next →</a>
</div>

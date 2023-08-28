## Section 3: Define Azure SQL Database Resources


# Table of Contents

- [Introduction](https://bitquip.github.io/Azure-SQL/1_introduction)
- [Prerequisites](https://bitquip.github.io/Azure-SQL/2_prerequisites)
- [Setup](https://bitquip.github.io/Azure-SQL/3_setup)
- [Define](https://bitquip.github.io/Azure-SQL/4_define)
- [Deploy](https://bitquip.github.io/Azure-SQL/5_deploy)
- [Cleanup](https://bitquip.github.io/Azure-SQL/6_cleanup)
- [Conclusion](https://bitquip.github.io/Azure-SQL/7_conclusion)


In this section, we'll define the resources needed to create an Azure SQL Database and its dependencies using Pulumi and TypeScript.

1. **Import Necessary Modules:** 

    Open the `index.ts` file in the `azure-ts` directory. At the top of the file, you'll need to import the required Pulumi and Azure modules:


```typescript
import * as vznAzNative from '@vizientinc/pulumi-azure-native';
import { ResourceGroupVzn } from '@vizientinc/pulumi-azure-native/lib/resources/ResourceGroupVzn';
import { authorization } from '@pulumi/azure-native';
import {Naming as VizNaming, sql} from '@vizientinc/pulumi-azure-native';
```

***ATTENTION:*** 

*Notice we are importing the Vizient Pulumi Azure module. This module is a wrapper around the official Pulumi Azure module.*

2. **Define Resources for Configuration:**
    
Next, we'll define some resources to hold configuration values.
These configuration values are common to all resources and will be referenced throughout the project.

- Resource Tags

```typescript
const tags = {
    env: 'sandbox',
    app: 'azure-sql',
    servicetier: 't1',
    owneremail: 'your_email@your_domain.com'
};
```

| Tag          | Description                                      |
|--------------|--------------------------------------------------|
| env          | The environment in which the resource is being created. This is typically one of the following values: sandbox, dev, qa, prod. |
| app          | The name of the application the resource is being created for. This is typically a string. |
| servicetier  | The service tier of the resource. This is typically one of the following values: t1, t2, t3, t4. |
| owneremail   | The email address of the resource owner. This is typically a string. |



Tags provide a way to associate metadata with Azure resources, such as virtual machines, storage accounts, databases, and more. 

These tags consist of name-value pairs and are particularly useful for organizing, categorizing, and managing resources within an Azure environment/subscription.

When creating resources at Vizient, we use a standard set of tags to help us organize and manage our resources. These tags are defined in the `tags` object above.

***Infrastructure Configuration***

```typescript
const infraConfigs = {
    subscriptionId: 'your_subscription_id'
    aseId: "",
    instance: '01',
    region: 'eastus2',
};
```

While the `tags` object is used to reference necessary values common to all resources, the `infraConfigs` object is used to reference values specific to the infrastructure configurations.


| Field         | Description                                                      |
|---------------|------------------------------------------------------------------|
| subscriptionId| The ID of the Azure subscription in which the resource is being created. This is typically a GUID. |
| aseId         | The ID of the Azure App Service Environment in which the resource is being created. This is typically a GUID. |
| instance      | The instance of the resource being created. This is typically a number. |
| region        | The Azure region in which the resource is being created. This is typically a string. |


3. **Define a Resource Group:**

    Start by defining a resource group to hold your Azure resources:

```typescript
new ResourceGroupVzn(`rg-eastus2-01`, {
        appName: tags.app,
        environment: tags.env,
        serviceTier: tags.servicetier,
        owner: vznAzNative.azuread.getUser({
            userPrincipalName: tags.owneremail,
        }),
        region: infraConfigs.region,
        instance: infraConfigs.instance,
        subscriptionId: infraConfigs.subscriptionId,
        devAADGroupName: roleConfigs.t1RoleGroupName,
    }).resourceGroup;
```

The `ResourceGroup` resource represents a logical container for your resources within Azure. It helps you manage your resources as a group and apply policies to them as a group.

In this code, we are creating a resource group named `rg-eastus2-01` in the `eastus2` region. We are also associating the resource group with the `azure-sql` application, the `sandbox` environment, and the `t1` service tier.

The `ResourceGroup` resource also allows you to apply policies to the resources within the group. In this code, we are associating the resource group with the `t1` role group. This role group is used to manage access to the resources within the resource group.

| Field              | Description                                                                                             |
|--------------------|---------------------------------------------------------------------------------------------------------|
| owner              | The owner of the resource group. This is typically a user object. The `getUser` method is used to retrieve the user object from Azure Active Directory. |
| devAADGroupName    | The name of the Azure Active Directory group that will be used to manage the resource group. This is typically a string. |


***NOTE:***

Typically you would define related resources within their own file and reference them in the main index.ts file. However, for the sake of simplicity, we'll define all of our resources directly in the index.ts file.

This will allow us to focus on the specific resource we're creating.

3. **Define an Azure SQL Server:**
    
    Now, let's define an Azure SQL Server:

```typescript
const sqlServer = new sql.ServerVzn(
  'sqlServer',
  {
    appInfo: {
      region: location,
      resourceGroupName: resourceGroup.name,
      serviceTier: serviceTier,
      tags: tags,
    },
    resourceName: VizNaming.SqlServer(app, env, location, instance),
    allowAllAzureTraffic: false,
    allowedVizientLocations: [
      LocationCode.cap,
      LocationCode.ae2,
    ],
    allowedSubnetIDs: vnetIDsSql2Allow,
    subscriptionId: subscriptionId,
    tenantId: tenantId,
  },
  {
    protect: false,
  }
);
```

In this code, we are creating an Azure SQL Server named `sqlServer` in the `eastus2` region. We are also associating the SQL Server with the `azure-sql` application, the `sandbox` environment, and the `t1` service tier.

The `Server` resource allows you to define the following properties:

| Field                  | Description                                                                                             |
|------------------------|---------------------------------------------------------------------------------------------------------|
| appInfo                | The application information associated with the SQL Server. This is typically an object containing the application name, environment, service tier, and tags. |
| resourceName           | The name of the SQL Server. This is typically a string. The `VizNaming.SqlServer` method is used to generate the name of the SQL Server. |
| allowAllAzureTraffic   | A boolean value indicating whether or not to allow all Azure traffic to the SQL Server. This is typically a boolean value. |
| allowedVizientLocations| An array of Azure locations that are allowed to access the SQL Server. This is typically an array of strings. |
| allowedSubnetIDs       | An array of Azure subnet IDs that are allowed to access the SQL Server. This is typically an array of strings. |
| subscriptionId         | The ID of the Azure subscription in which the SQL Server is being created. This is typically a GUID. |
| tenantId               | The ID of the Azure Active Directory tenant in which the SQL Server is being created. This is typically a GUID. |



4. **Create an Azure SQL Database:** Next, define an Azure SQL Database within the SQL Server:

```typescript
 new sql.DatabaseVzn(
  'sqlDatabase',
  {
    createAADGroupsInSandbox: true,
    resourceName: 'sqldatabase',
    server: sqlServer.sqlServer, // notice we are using the sqlServer object created above
    databaseSku: {
      name: daasDbDTU,
    },
    maxSizeInGiB: 100,
    appInfo: {
      region: location,
      resourceGroupName: resourceGroup.name,
      tags: tags,
      serviceTier: serviceTier,
    },
  },
  {
    parent: sqlServer.sqlServer,
    dependsOn: sqlServer.sqlServer,
  }
);
```

In this code, we are creating an Azure SQL Database named `sqlDatabase` in the `eastus2` region. We are also associating the SQL Database with the `azure-sql` application, the `sandbox` environment, and the `t1` service tier.

The `Database` resource allows you to define the following properties:  

| Field                      | Description                                                                                             |
|----------------------------|---------------------------------------------------------------------------------------------------------|
| createAADGroupsInSandbox   | A boolean value indicating whether or not to create Azure Active Directory groups in the sandbox environment. This is typically a boolean value. |
| resourceName               | The name of the SQL Database. This is typically a string. |
| server                     | The SQL Server in which the SQL Database is being created. This is typically a SQL Server object. Notice that we are using the `sqlServer` object created above. |
| databaseSku                | The SKU of the SQL Database. This is typically an object containing the name of the SKU. The `daasDbDTU` variable is used to define the name of the SKU. |
| maxSizeInGiB               | The maximum size of the SQL Database in gigabytes. This is typically a number. |
| appInfo                    | The application information associated with the SQL Database. This is typically an object containing the application name, environment, service tier, and tags. |



<div style="display: flex; justify-content: space-between; align-items: center;">
    <a href="https://bitquip.github.io/Azure-SQL/3_setup" style="margin: 10px; text-decoration: none;">← Previous</a>
    <a href="https://bitquip.github.io/Azure-SQL/5_deploy" style="margin: 10px; text-decoration: none;">Next →</a>
</div>

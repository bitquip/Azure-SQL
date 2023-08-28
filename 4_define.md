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

- Infrastructure Configuration

```typescript
const infraConfigs = {
    subscriptionId: 'your_subscription_id'
    aseId: "",
    instance: '01',
    region: 'eastus2',
};
```

The `tags` object is used to reference necessary values common to all resources. 

The `infraConfigs` object is used to reference values specific to the infrastructure configurations.

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

In this code, ...

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

Here, you're creating an Azure SQL Database named "sqldatabase" on the SQL Server you've just created. 


<div style="display: flex; justify-content: space-between; align-items: center;">
    <a href="https://bitquip.github.io/Azure-SQL/3_setup" style="margin: 10px; text-decoration: none;">← Previous</a>
    <a href="https://bitquip.github.io/Azure-SQL/5_deploy" style="margin: 10px; text-decoration: none;">Next →</a>
</div>

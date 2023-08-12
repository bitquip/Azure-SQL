## Section 4: Deploy the Resources

With the resources defined, it's time to deploy them:

1. **Save and Deploy:** Save the changes you made to the `index.ts` file.

2. **Deploy the Resources:** In your terminal, navigate to the root of your project directory and run the following command:

```bash
pulumi up
```

Pulumi will analyze your code, create a deployment plan, and prompt you to confirm the changes before proceeding.

3. **Review and Confirm:** After reviewing the proposed changes, type `yes` to confirm and initiate the deployment.

4. **Observe Outputs:** Once the deployment is complete, Pulumi will display outputs, including connection strings for the SQL Server and Database. These outputs are crucial for connecting to your newly created resources.

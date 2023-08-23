## Section 4: Deploy the Resources


# Table of Contents

- [Introduction](https://bitquip.github.io/Azure-SQL/1_introduction.md)
- [Prerequisites](https://bitquip.github.io/Azure-SQL/2_prerequisites.md)
- [Setup](https://bitquip.github.io/Azure-SQL/3_setup.md)
- [Define](https://bitquip.github.io/Azure-SQL/4_define.md)
- [Deploy](https://bitquip.github.io/Azure-SQL/5_deploy.md)
- [Cleanup](https://bitquip.github.io/Azure-SQL/6_cleanup.md)
- [Conclusion](https://bitquip.github.io/Azure-SQL/7_conclusion.md)


With the resources defined, it's time to deploy them:

1. **Save and Deploy:** Save the changes you made to the `index.ts` file.

2. **Deploy the Resources:** In your terminal, navigate to the root of your project directory and run the following command:

```bash
pulumi up
```

Pulumi will analyze your code, create a deployment plan, and prompt you to confirm the changes before proceeding.

3. **Review and Confirm:** After reviewing the proposed changes, type `yes` to confirm and initiate the deployment.

4. **Observe Outputs:** Once the deployment is complete, Pulumi will display outputs, including connection strings for the SQL Server and Database. These outputs are crucial for connecting to your newly created resources.

<div style="display: flex; justify-content: space-between; align-items: center;">
    <a href="https://bitquip.github.io/Azure-SQL/4_define.md" style="margin: 10px; text-decoration: none;">← Previous</a>
    <a href="https://bitquip.github.io/Azure-SQL/6_cleanup.md" style="margin: 10px; text-decoration: none;">Next →</a>
</div>

# Azure Policy

Azure Policy helps you manage and prevent IT issues with policy definitions that enforce rules and effects for your resources. When you use Azure Policy, resources stay compliant with your corporate standards and service level agreements.

In this lab we are going to create a policy that will report when the tag  `department` is not set to `lab`.

## Exercise 1 - Create a Policy

1. Launch the Azure Policy service in the Azure portal by clicking **All services**, then searching for and selecting **Policy**. Click on the star and **Policy** will now appear on the left pane.  Select **Policy**.
2. Select **Assignments** under **Authoring** on the left side of the Azure Policy page. An assignment is a policy that has been assigned to take place within a specific scope.
3. Select **Assign Policy** from the top of the **Policy | Assignments** page.
4. On the **Basic** page:
    * Select the ellipsis button for **Scope** and then choose your subscription and then set the Resource Group to **Migration**.  Click **Select**.
    * Select the **Policy definition** ellipsis button to open the list of available definitions.
    * Search for **tag**, select **Require tag and its value on resources**, and click **Select**.
5. Select the **Parameters** tab and enter the following:
    * Tag name: **department**
    * Tag value: **lab**
6. On the **Remediation** tab select **Create a Managed Identity**.
7. Click **Review + create** and then **Create**.

## Exercise 2 - Create and assign an initiative definition

An initiative evaluates resources within scope of the assignment for compliance to the included policies.

1. Select **Definitions** under Authoring in the left side of the Azure Policy page.
2. Select **+ Initiative Definition** at the top of the page to open the Initiative definition page.
3. Click on the ellipsis button for **Definition location**, choose your Azure subscription, and then click **Select**.
3. Enter **Tag Enforcement** as both the Name and Description of the initiative.
4. For Category, choose *Create new*, and enter **MyFirstInitiative**.
5. Under **AVAILABLE DEFINITIONS** search for *tag* and then select **Require tag and its value on resources**, the **+Add**.
6. **Set value** of `Tag Name` to **department**.
7. **Set value** of `Tag Value` to **lab**.
8. Click **Save**.

## Exercise 3 - Check initial compliance

Now that we've created and applied a policy let's check the results on how compliant we are.  Please note that it may take Azure Policy up to `10 minutes` to evaluate and report results.

1. Select **Compliance** in the left side of the Azure Policy page.
2. Locate the **Require tag and its value on resources** initiative. It's likely still in Compliance state of `Not started` or `Non-compliant`. Click on the initiative to get full details on the progress of the assignment.
3. Hit **Refresh** periodically in order to check the **Compliance state**.

## Exercise 4 - Modify Non-compliant resources

Once evaluations are returned, you will notice all of your resources are non-compliant.  Complete these steps to make one resource compliant and review the results.

1. Click on **Require tag and its value**.
2. Under resource compliance, click on any resource and then **view resource**.
3. Under Tags click **click here to add tags**.
4. Set `Tag Name` to **department**.
5. Set `Tag Value` to **lab**.
6. Click **Save**
7. On the left-hand side click **Policy** and then **Compliance**.
8. Notice the count on the Overall resource compliance on **Require tag and its value**.  Hit refresh periodically to see the values change.
9. You can click on **Require tag and its value** to see the time when the resource was last evaluated.

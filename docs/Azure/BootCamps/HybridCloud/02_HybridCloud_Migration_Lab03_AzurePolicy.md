# Azure Migration Lab

## Azure Policy

Azure Policy helps you manage and prevent IT issues with policy definitions that enforce rules and effects for your resources. When you use Azure Policy, resources stay compliant with your corporate standards and service level agreements.

In this lab we are going to create a policy that will report when the tag  `department` is not set to `lab`.

## Create a Policy

1. Launch the Azure Policy service in the Azure portal by clicking **All services**, then searching for and selecting **Policy**. Click on the star and **Policy** will now appear on the left pane.  Select **Policy**.
2. Select **Assignments** under **Authoring** on the left side of the Azure Policy page. An assignment is a policy that has been assigned to take place within a specific scope.
3. Select **Assign Policy** from the top of the **Policy - Assignments** page.
4. On the **Assign Policy** page, select the **Policy definition** ellipsis to open the list of available definitions.
5. Seach for **tag**, select **Require tag and its value**, and click **Select**.
6. Select the **Parameters** tab and enter the following:
    * Tag name: **department**
    * Tag value: **lab**
7. Click **Review + create** and then **Create**.

## Create and assign an initiative definition

An initiative evaluates resources within scope of the assignment for compliance to the included policies.

1. Select **Definitions** under Authoring in the left side of the Azure Policy page.
2. Select **+ Initiative Definition** at the top of the page to open the Initiative definition page.
3. Enter **Tag Enforcement** as both the Name and Description of the initiative.
4. For Category, choose *Create new*, and enter **MyFirstInitiative**.
5. Under **AVAILABLE DEFINITIONS** search for *tag* and then select **Enforce tag and its value**, the **+Add**.
6. **Set value** of `Tag Name` to **department**.
7. **Set value** of `Tag Value` to **lab**.
8. Click **Save**.

## Check initial compliance

Now that we've created and applied a policy let's check the results on how compliane we are.  Please note that it may take Azure Policy up to `10 minutes` to evaluate and report results.

1. Select **Compliance** in the left side of the Azure Policy page.
2. Locate the **Require tag and its value** initiative. It's likely still in Compliance state of `Not started` or `Non-compliant`. Click on the initiative to get full details on the progress of the assignment.

## Modify Non-compliant resources
Once evaluations are returned, you will notice all of your resources are non-compliant.  Complete these steps to make one resource compliant and review the results.

1. Click on **Require tag and its value**.
2. Under resource compliance, click on **SQLVM** and then **view resource**.
3. Under Tags click **click here to add tags**.
4. Set `Tag Name` to **department**.
5. Set `Tag Value` to **lab**.
6. Click **Save**
7. On the left-hand side click **Policy** and then **Compliance**.
8. Notice the count on the Overall resource compliance on **Require tag and its value**.  Hit refresh periodically to see the values change.
9. You can click on **Require tag and its value** to see the time when the resource was last evaluated.

[Back](index.md)

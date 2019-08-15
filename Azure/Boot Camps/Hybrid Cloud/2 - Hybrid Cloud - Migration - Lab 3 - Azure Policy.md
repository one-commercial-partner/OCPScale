# Azure Migration Lab

## Azure Policy
Azure Policy helps you manage and prevent IT issues with policy definitions that enforce rules and effects for your resources. When you use Azure Policy, resources stay compliant with your corporate standards and service level agreements.

Understanding how to create and manage policies in Azure is important for staying compliant with your corporate standards and service level agreements. In this tutorial, you learn to use Azure Policy to do some of the more common tasks related to creating, assigning, and managing policies across your organization, such as:
* Assign a policy to enforce a condition for resources you create in the future
* Create and assign an initiative definition to track compliance for multiple resources
* Resolve a non-compliant or denied resource
* Implement a new policy across an organization*

### Create a Policy
1. Launch the Azure Policy service in the Azure portal by clicking **All services**, then searching for and selecting **Policy**. Click on the star and **Policy** will now appear on the left pane.  Select **Policy**.
2. Select Assignments under **Authoring** on the left side of the Azure Policy page. An assignment is a policy that has been assigned to take place within a specific scope.
3. Select **Assign Policy** from the top of the **Policy - Assignments** page.
4. On the **Assign Policy **page, select the** Policy definition** ellipsis to open the list of available definitions. 
5. Seach for and select **Enforce tag and its value**. 
6. Enter the following under **PARAMETERS** and then click **Assign**
    * Tag name: **department**
    * Tag value: **lab**

### Create and assign an initiative definition
With an initiative definition, you can group several policy definitions to achieve one overarching goal. An initiative evaluates resources within scope of the assignment for compliance to the included policies. 

#### Create an initiative definition
1. Select **Definitions** under Authoring in the left side of the Azure Policy page.
2. Select **+ Initiative Definition** at the top of the page to open the Initiative definition page.
3. Enter **Tag Enforcement** as the Name and Description of the initiative.
4. For Category, choose *Create new*, and enter **MyFirstInitiative**.
5. Under **AVAILABLE DEFINITIONS** search for *tag* and then select **Enforce tag and its value**, the **+Add**.
6. **Set value** of `Tag Name` to **department**.
7. **Set value** of `Tag Value` to **lab**.
8. Click **Save**.

#### Check initial compliance
1. Select **Compliance** in the left side of the Azure Policy page.
2. Locate the **Enforce tag and its value** initiative. It's likely still in Compliance state of `Not started` or `Non-compliant`. Click on the initiative to get full details on the progress of the assignment.


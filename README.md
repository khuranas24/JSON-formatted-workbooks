**Programmatically manage workbooks**

Resource owners can create and manage their workbooks programmatically via Azure Resource Manager templates (ARM templates).

This capability can be useful in scenarios like:

Deploying org- or domain-specific analytics reports along with resources deployments. For instance, you can deploy org-specific performance and failure workbooks for your new apps or virtual machines.
Deploying standard reports or dashboards by using workbooks for existing resources.
The workbook will be created in the desired sub/resource-group and with the content specified in the ARM templates.

Two types of workbook resources can be managed programmatically:

- Workbook templates
- Workbook instances

**ARM template for deploying a workbook template**
- Open a workbook you want to deploy programmatically.

- Switch the workbook to edit mode by selecting Edit.

- Open the Advanced Editor by using the </> button on the toolbar.

- Ensure you're on the Gallery Template tab.

- Screenshot that shows the Gallery Template tab.

- Copy the JSON in the gallery template to the clipboard.

The following sample ARM template deploys a workbook template to the Azure Monitor workbook gallery. Paste the JSON you copied in place of <PASTE-COPIED-WORKBOOK_TEMPLATE_HERE>

**Work with JSON-formatted workbook data in the serializedData template parameter**

When you export an ARM template for an Azure workbook, there are often fixed resource links embedded within the exported serializedData template parameter. These links include potentially sensitive values such as subscription ID and resource group name, and other types of resource IDs.

The following example demonstrates the customization of an exported workbook ARM template, without resorting to string manipulation. The pattern shown in this example is intended to work with the unaltered data as exported from the Azure portal. It's also a best practice to mask out any embedded sensitive values when you manage workbooks programmatically. For this reason, the subscription ID and resource group have been masked here. No other modifications were made to the raw incoming serializedData value.

In this example, the following steps facilitate the customization of an exported ARM template:

- Export the workbook as an ARM template as explained in the preceding section.
- In the template's variables section:
- Parse the serializedData value into a JSON object variable, which creates a JSON structure including an array of items that represent the content of the workbook.
- Create new JSON objects that represent only the items/properties to be modified.
- Project a new set of JSON content items (updatedItems) by using the union() function to apply the modifications to the original JSON items.
- Create a new workbook object, updatedWorkbookData, that contains updatedItems and the version/isLocked data from the original parsed data and a corrected set of fallbackResourceIds.
- Serialize the new JSON content back into a new string variable, reserializedData.
- Use the new reserializedData variable in place of the original serializedData property.
- Deploy the new workbook resource by using the updated ARM template.

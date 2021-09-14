# externaldata project
author: Nathan Swift

The following project will provide the example externaldata()[] KQL queries and schema to use agaisnt Azure Storage, where Data Export rules are sending the Azure Sentinel logs to for long term retention.

To leverage the solution create a Azure storage account where you will store long term retention security logs into. Create and deploy a data export rule to azure storage onto the Log analytics workspace, updating the deplyment template to include the table names that need to have the logs stored in log term retention.

[Data Export ARM Template](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/logs-data-export?tabs=json#create-or-update-data-export-rule)


FUTURES:
1. Build a .ps1 script to enter table name, dates to start and stop to lookup, and storage account where stored. Script then enumerates and generates SAS signatures for each .json blob. Script then will print and generate the specific KQL query to use. 

2. create parallelism in script to improve performance.

3. Additional function to then prompt for a custom kql query input after the externaldata lookup and execute kql query and export results in a csv format.

4. error handling for schema not found - create generic kql query instead
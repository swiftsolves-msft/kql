# externaldata project
author: Nathan Swift

The following project will provide the example externaldata()[] KQL queries to use agaisnt Azure Storage where Data Export rules are sending the Azure Sentinel logs to for long term retention.

FUTURES:
-Build a .ps1 script to enter table name, dates to start and stop to lookup, and storage account where stored. Script then enumerates and generates SAS signatures for each .json blob. Script then will print and generate the specific KQL query to use. 
-Additional function to then prompt for a custom kql query input after the externaldata lookup and execute kql query and export results in a csv format.
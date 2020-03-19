Azure Data Factory has added support for MSI, this MSI can be used to invoke processing of an Azure Analysis Services database using a Web Activity.

To set this processing activity in ADF, gather the below listed items from your environment.



Sl#|Item | Sample Value | How to get it
------|-----|--------------|-------------
1|AAD Directory ID |99999999-86f1-41af-9999-2d7cd011db47| [Screenshot 1](/step1_getdirectory%20id.jpg)   |
2|ADF - Managed Identity App ID |88888888-18de-407d-8888-a75feab53f82|[Screenshot 2](/step2_get%20managedidentityappid.jpg)
3|Azure AS Server Name|asazure://southcentralus.asazure.windows.net/azureasshaka01|Azure Portal/SSMS
4|Azure AS Database Name|AzureASProcessing| Azure Portal/SSMS
  
    
      
  
  1. Grant Permissions for the ADF MSI on Azure Analysis Services using SSMS.  Please follow the below syntax and add the MSI as a 'server admin'
  
  Syntax 
  ```
  app:<ADF - Managed Identity App ID>@<AAD Directory ID>
  ```
  
  Sample
  ```
  app:88888888-18de-407d-8888-a75feab53f82@99999999-86f1-41af-9999-2d7cd011db47
  ```
  ![SSMS Screenshot](/step3_addmsitoazureasfromssms.jpg)
  
  2. Create a pipeline in ADF, add a "Web" activity to the pipeline and set the below properties, replace the highlighted items with your azure region,azure as server name and database name
  
  Property name | Sample Value
  ------|----------
  URL | https://`southcentralus`.asazure.windows.net/servers/`azureasshaka01`/models/`AzureASProcessing`/refreshes
  Method|Post
  Body|{"type":"full","objects":[{"database":"`AzureASProcessing`"}]}
  Authentication| MSI
  Resource|https://*.asazure.windows.net
  
  ![ADF Screenshot](/step4_addwebtaskinadf.jpg)


Additional Links  
  
  
https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity  
https://docs.microsoft.com/en-us/azure/data-factory/control-flow-web-activity


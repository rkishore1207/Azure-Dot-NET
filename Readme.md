# AZURE WITH DOT NET

#### Integrate Local Powershell prompt with Azure Account
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

> Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force 
* If the previous command is throwing error, then use below
> Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force -AllowClobber
* Then use `Connect-AzAccount`, it will show a popup window to select desired account.
* If our account have multiple **Subscription** then, it will ask us to select under which subscription we are going to work.
-------
#### Azure Powershell (Both cloud and Local)
* $resourceGroup = "powershell-resource-group"
* $location = "eastus"
* `New-AzResourceGroup -Name $resourceGroup -location $location`
* To view, **Get-AzResourceGroup**
* To list Locations, **Get-AzLocation | Select DisplayName**

#### Azure Cloud Shell (Bash)
* To List Locations, **az account list-locations --query "[].{Region:name}" --out table**

#### Azure CLI (Local)
* To login, **az login**
* It will authenticate and logged in to our Azure portal.
* To view all subscription, **az account list --output table**
* To configure to our Subscription (kiramesh), Copy our subscription's ID from the list of subscriptons and write **az account set --subscription "Paste Subscription Id"**
* To view our mapped subscription account, **az account show --output table**
* To Create Resource Group, **az group create --name "cli-resource-group" --location "eastus"**
* To view list of Resource Group, **az group list**

## App Service with Dot NET

![App Service](https://github.com/rkishore1207/Azure-Dot-NET/assets/146698138/bd9137c5-78d9-468d-9ba9-0c11ebb72194)

### Deploy WebApplication to Azure with CLI
```
* dotnet new webapp -o first-deploy-app
* dotnet build
* dotnet publist -o Publish
* cd .\Publish\
* Compress * Publish.zip (* -> all file in that folder converted to zip in the name of Publish.zip)
* Create our own Resource Group, AppService Plan and WebApp to deploy our DotNet project.
* Resource Group -> az group create --location eastus --resource-group kishore-resource-group
* AppService Plan -> az appservice plan create -g kishore-resource-group -n kishore-appservice-plan
* Webapp -> az webapp create -g kishore-resource-group -p kishore-appservice-plan -n kishore-webapp
* Then deploy -> az webapp deployment source config-zip --src .\Publish.zip\ -n kishore-webapp -g kishore-resource-group
* Open -> az webapp browse -g kishore-resource-group -n kishore-webapp
```

### Azure WebApp Logging
* First Dowload the Extension for the Webapp, `ASP.NET Core Logging` Extension.
* Go to the Logging -> Click `App Service Logs`, and Enable **Application Logging(File System)** to `Verbose` (OS can log all the errors from the Device)
* We can see, all the logs on the Command Prompt.

### App Settings Configuration
* At our Api, add sample key and value at appsettings.json and consume.
* It will vary depends upon our value at Azure.
* We can view our application in Azure  thorugh **App Service Editor (Preview)**
* Go to Configuration, click add App Settings, give the key and value for the **Production Environment**, and this value will be override at the Browser.
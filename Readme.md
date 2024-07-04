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


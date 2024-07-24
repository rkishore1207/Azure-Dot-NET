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
* dotnet publish -o Publish
* cd .\Publish\
* Compress-Archive * Publish.zip (* -> all file in that folder converted to zip in the name of Publish.zip)
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

### Deployment Slots
* It is a another **Webapp** that was clonned from the Webapp which was deployed in the **Production environment**.
* We need to create separate `Slot` for the new Webapp under the **same plan**.
* For **Stage**, at git-hub create new branch, and configure that branch in the newly created *stage slot (at deployment center)*.
* Once it was done it will hosted as a new Webapp, this is for check previously if our application behave correctly, before it went into the Production.
* If everything is work fine, then merge that changes into develop branch and it will trigger the deployment automatically and our changes will move into Production too.

## Azure SQL Database
* Create Azure SQL Database in the Portal.
* We need to create `SQL Server` (Like Appservice plan for Webapp), we have some configuration, based on our requirement we would choose the package of SQL Server.
* And Choose Authentication from **SQL Authentication** (if our code have **Azure AD** then Select both), here give `User Login and Password` for our Database.
* We have to set **SET SERVER FIREWALL** -> Select **Selected Networks**, add **new Firewall rules**, copy our local `SSMS's IP address` and paste here then create, as same as create needed firewall rules, with add the `default one` from the Azure portal itself `(default IP address)`.
* Check Allow **Azure Services** Checkbox.
* Copy the Connection Strings (ADO.NET)
* In API, create Connection Strings Key at **app.development.json** file, the same we need to configure in the Azure WebApp, *Configurtion -> Add new Connection String -> paster the key and value and type is Azure SQL*. Then only it would reflect in the Production's App.
```
* dotnet tool install --global dotnet-ef
* dotnet add package Microsoft.EntityFrameworkCore.Design
* dotnet add package Microsoft.EntityFrameworkCore.SqlServer
* Configure Migrations for EF -> dotnet ef Migrations add Initial
* dotnet ef database update
* By using Scaffold Command, create new page -> dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
* dotnet add package Microsoft.EntityFrameworkCore.Tools
* dotnet aspnet-codegenerator razorpage -m Person -dc AzureDbContext -udl -outDir Pages/Persons --referenceScriptLibraries
```

## Azure Cosmos DB
* It is also a **Database** but we don't need any server to do Queries, in the cloud itself it has Database and we **will do Query there itself**.
* We have to create `Azure-cosmos-account`, and need to create Databases, Containers and items (optional).
* Containers are **Tables** and Items are **Records**.
* We have to connect this Credential to our code by ConnectionString (URI + Primary key) and DB Name and Container Name by Cosmos Client.
* For Git hub `Workflow`, we can configure **our root folder** by adding that folder in the end of each step.
* We cannot push directly to github with Secret keys
* We can put those keys on `Environment variables` through Right click on Project -> Debug -> General -> launch -> Add New Envionment variables.
* Or Add those in the **command prompt** itself.

## Azure Storage Account

![Storage Account](https://github.com/user-attachments/assets/e4bcc1a3-d660-4931-baff-b32446671f79)

## Azure Service Bus
* It is a **Message broker**, which holds `message` and some other applications are trigger by using this message.
* **Storage Queue** also a Message broker.
* Message broker helps to transfer business related data between applications.
* It ensures **Decoupling** between applications.
* **Reliability** -> if one application goes down, message broker stores the message, once that application is available that can take the message from broker.
* **Scalability** -> don't hold the unnecessary message in the queue, if that message is read then it will delete it from the queue.
* It is **PaaS**.
* If some messages aren't read properly, that will stored in the **Dead letter Queue**.

## Azure Serverless Function

![Azure Functions](https://github.com/user-attachments/assets/4ed9f2dc-f341-4eef-8c6e-4e5674ea1e48)

* All the internet are came from server only but, as a developer we don't worry about the server, just we have to give solutions to the problems.
* It is an event-driven service **(event router)** to make a communication between **producer** and **consumer**.

### Triggers and Bindings

![Triggers and Bindings](https://github.com/user-attachments/assets/9aa377c3-b51e-40e5-a4f9-ddd8b34f675a)

--------------
### Durable Functions
* Regularly functions doesn't store any state in it, so if one function stores the incoming states in it, then its called Durable Function.
* This will done by passing function as a parameter to another function (Passing function is **Orchestrator** and called function is **Entity**).
* There are two patterns for Durable Functions
1. **Function Chaining**
- Passing function into another function to store state.
- One output can be used as a input for another function.
2. **FanIn - FanOut**
- Executing all functions parallely, to store the state.
- But if all the functions are executed completely, then only it will give output.

> Accepted Response -> That request may or may not complete but now we are good to go
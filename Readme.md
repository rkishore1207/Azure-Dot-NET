# AZURE WITH DON NET

#### Integrate Local Powershell prompt with Azure Account
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

> Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force 
* If the previous command is throwing error, then use below
> Install-Module -Name Az -Scope CurrentUser -Repository PSGallery -Force -AllowClobber
* Then use `Connect-AzAccount`, it will show a popup window to select desired account.
* If our account have multiple **Subscription** then, it will ask us to select under which subscription we are going to work.
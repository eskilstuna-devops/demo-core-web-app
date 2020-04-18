# Demo Core Web App

## This demo includes a Azure DevOps pipeline that builds and deploys a core webapp to Azure cloud.

### Prerequisites

- Azure Subscription
- Azure DevOps Organization 
- GitHub Organization

### Azure Environments
- demo
- dev
- test
- prod


Login to your Azure account using Azure CLI. The command below takes you to a weblogin in a browser.
```powershell
az login
```

Create the resource group 
```powershell
az group create --name rg-demo-weu --location westeurope
```

Create the resource appservice plan
```powershell
az appservice plan create --name asp-demo --resource-group rg-demo-weu --sku FREE
```

Create the resource web app
```powershell
az webapp create --name wa-top-demo --resource-group rg-demo-weu --plan asp-demo
```

### Azure DevOps pipelines


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

```yaml
# ASP.NET Core
# Build and test ASP.NET Core projects targeting .NET Core.
# Add steps that run tests, create a NuGet package, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/dotnet-core

trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

steps:
- script: dotnet build --configuration $(buildConfiguration)
  displayName: 'dotnet build $(buildConfiguration)'

- task: DotNetCoreCLI@2
  inputs:
    command: 'publish'
    publishWebProjects: true
    arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
    publishLocation: 'Container'
```
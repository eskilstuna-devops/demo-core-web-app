# Demo Core Web App

[![GitHub version](https://badge.fury.io/gh/tpeltone%2demo-core-web-app.svg)](https://badge.fury.io/gh/tpeltone%2demo-core-web-app) 

<!-- [![GitHub version](https://badge.fury.io/gh/conventional-changelog%2Fstandard-version.svg)](https://badge.fury.io/gh/conventional-changelog%2Fstandard-version) -->

[![Build Status](https://dev.azure.com/tomipeltonen/azure-labs/_apis/build/status/eskilstuna-devops-ci.demo-core-web-app?branchName=master)](https://dev.azure.com/tomipeltonen/azure-labs/_build/latest?definitionId=27&branchName=master)

This demo includes a Azure DevOps pipeline that builds and deploys a asp.net core webapp to Azure cloud app service.

## Links 
[Continuous Integration, Continuous Deployment (CI-CD) with Azure DevOps](https://www.youtube.com/watch?v=jRgLSMlp28U)

### Prerequisites

- Azure Subscription
- Azure DevOps Organization 
- GitHub Organization

### Create anotated tag for the web app

Get the latest commits SHA-1
```bash
git log --oneline --graph --decorate
```
Tag
```bash
git tag -a -m 'The first v1.0.0' v1.0.0 b38d91e
```

Push
 ```bash
git push origin --follow-tags
 ```

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
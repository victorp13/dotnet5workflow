## .NET 5 workflow, publish to Azure Web Apps

This example .NET 5 Webapp project has a Github Actions [workflow file](.github/workflows/dotnet5.yml) that builds, publishes & deploys to an Azure Web App.

The workflow file (shown below) requires a Githubs SECRET on the repository with the Publish Profile of the Azure Web App.

```name: .NET 5

on:
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Setup .NET 5
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 5.0.100
    - name: Build
      run: dotnet build --configuration Release
    - name: dotnet publish
      run:  dotnet publish -c Release -o dotnet5webapp 
    - name: Azure WebApp
      uses: Azure/webapps-deploy@v2
      with:
        app-name: dotnet5workflow
        publish-profile: ${{ secrets.AZURE_APP_SERVICE_PUBLISHSETTINGS }}
        package: './dotnet5webapp' 

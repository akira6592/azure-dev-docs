---
title: Create Azure Monitor resource
description: Create an Azure resource group for all your Azure resources and a Monitor resource to collect your web app's log files to the Azure cloud. Azure Monitor is the name of the Azure service, while Application Insights is the name of the client library the tutorial uses.
ms.topic: how-to
ms.date: 01/18/2022
ms.custom: devx-track-js, devx-track-azurecli
---

# 2. Create Application Insights resource for web pages

In this step of the tutorial, create an Azure resource group for all your Azure resources and a Monitor resource to collect your web app's log files to the Azure cloud. Azure Monitor is the name of the Azure service, while Application Insights is the name of the client library the tutorial uses. 

## Create a resource group for your virtual machine resources

This tutorial includes several Azure resources. Creating a resource group allows you to easily find the resources, and delete them when you are done.

At a terminal or bash shell, enter the Azure CLI command to create an Azure resource group, [az group create](/cli/azure/group#az_group_create), with the name `rg-demo-vm-eastus`:

```azurecli
az group create \
    --location eastus \
    --name rg-demo-vm-eastus 
```

## Create Azure Monitor resource with Azure CLI

1. Install Application Insights extension to the Azure CLI.

    ```azurecli
    az extension add -n application-insights
    ```

1. Use the following command to create a monitoring resource, with [az monitor app-insights component create](/cli/azure/monitor/app-insights/component#az_monitor_app_insights_component_create):


    ```azurecli
    az monitor app-insights component create \
      --app demoWebAppMonitor \
      --location eastus \
      --resource-group rg-demo-vm-eastus \
      --query instrumentationKey --output table
    ```

    In the resulting table, copy the **Result**, you will need that value as your `instrumentationKey` later. 

1. Leave the terminal open, you will use it in the next step.

## Next step

> [!div class="nextstepaction"]
> [Create Linux virtual machine](create-linux-virtual-machine-azure-cli.md) 

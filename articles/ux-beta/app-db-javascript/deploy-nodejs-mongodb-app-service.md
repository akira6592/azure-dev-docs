---
title: Deploy a Node.js web app using MongoDB to Azure
description: This article shows you have to deploy a Node.js app using Express.js and a MongoDB database to Azure.  Azure App Service is used to host the web application and Azure Cosmos DB to host the database using the 100% compatible MongoDB API built into Cosmos DB. 
ms.topic: tutorial
ms.date: 01/07/2022
ms.service: app-service
ms.role: developer
ms.devlang: javascript
ms.azure.dev-framework: expressjs
ms.azure.devx-azure-tooling: ['azure-portal', 'vscode-azure-tools', 'azure-cli']
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-js
ROBOTS: NOINDEX
---

# Deploy a Node.js + MongoDB web app to Azure

In this tutorial, you'll deploy a sample **Express.js** app using a **MongoDB** database to Azure.  The Express.js app will be hosted in Azure App Service which supports hosting Node.js apps in both Linux (Node versions 12, 14, and 16) and Windows (versions 12 and 14) server environments. The MongoDB database will be hosted in Azure Cosmos DB, a cloud native database offering a [100% MongoDB compatible API](/azure/cosmos-db/mongodb/mongodb-introduction).

:::image type="content" source="./media/nodejs-mongodb/app-diagram.png" alt-text="A diagram showing how the Express.js app will be deployed to Azure App Service and the MongoDB data will be hosted inside of Azure Cosmos DB." lightbox="./media/nodejs-mongodb/app-diagram-large.png":::

This article assumes you are already familiar with [Node.js development](/learn/paths/build-javascript-applications-nodejs/) and have Node and MongoDB installed locally. You'll also need an Azure account with an active subscription.  If you do not have an Azure account, you [can create one for free](https://azure.microsoft.com/free/nodejs/).

## Sample application

To follow along with this tutorial, clone or download the sample application from the repository [https://github.com/Azure-Samples/msdocs-nodejs-mongodb-azure-sample-app](https://github.com/Azure-Samples/msdocs-nodejs-mongodb-azure-sample-app).

```bash
git clone https://github.com/Azure-Samples/msdocs-nodejs-mongodb-azure-sample-app.git
```

Follow these steps to run the application locally:

* Change directories into the `src` folder
* Install the package dependencies by running `npm install`
* Copy the `.env.sample` file to `.env` and populate the DATABASE_URL value with your MongoDB URL (for example *mongodb://localhost:27017/*)
* Start the application using `npm start`
* To view the app, browse to `http://localhost:3000`

## 1 - Create the Azure App Service

Azure App Service is used to host the Express.js web app. When setting up the App Service for the application, you will specify:

* The **Name** for the web app.  This name is used as part of the DNS name for your webapp in the form of `https://<app-name>.azurewebsites.net`.
* The **Runtime** for the app.  This is where you select the version of Node to use for your app.
* The **App Service plan** which defines the compute resources (CPU, memory) available for the application.
* The **Resource Group** for the app.  A resource group lets you group all of the Azure resources needed for the application together in a logical container.

Azure resources can be created using the [Azure portal](https://portal.azure.com/), VS Code using the [Azure Tools extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack), or the Azure CLI.

### [Azure portal](#tab/azure-portal)

Sign in to the [Azure portal](https://portal.azure.com/) and follow these steps to create your Azure App Service resources.

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Create app service step 1](<./includes/nodejs-mongodb/create-app-service-azure-portal-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-azure-portal-1-240px.png" alt-text="A screenshot showing how to use the search box in the top tool bar to find App Services in Azure." lightbox="./media/nodejs-mongodb/create-app-service-azure-portal-1.png"::: |
| [!INCLUDE [Create app service step 2](<./includes/nodejs-mongodb/create-app-service-azure-portal-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-azure-portal-2-240px.png" alt-text="A screenshot showing the create button on the App Services page used to create a new web app." lightbox="./media/nodejs-mongodb/create-app-service-azure-portal-2.png"::: |
| [!INCLUDE [Create app service step 3](<./includes/nodejs-mongodb/create-app-service-azure-portal-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-azure-portal-3-240px.png" alt-text="A screenshot showing the form to fill out to create a web app in Azure." lightbox="./media/nodejs-mongodb/create-app-service-azure-portal-3.png"::: |
| [!INCLUDE [Create app service step 4](<./includes/nodejs-mongodb/create-app-service-azure-portal-4.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-azure-portal-4-240px.png" alt-text="A screenshot of the Spec Picker dialog that allows you to select the App Service plan to use for your web app." lightbox="./media/nodejs-mongodb/create-app-service-azure-portal-4.png"::: |
| [!INCLUDE [Create app service step 4](<./includes/nodejs-mongodb/create-app-service-azure-portal-5.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-azure-portal-5-240px.png" alt-text="A screenshot of the main web app create page showing the button to select on to create your web app in Azure." lightbox="./media/nodejs-mongodb/create-app-service-azure-portal-5.png"::: |

### [VS Code](#tab/vscode-aztools)

To create Azure resources in VS Code, you must have the [Azure Tools extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) installed and be signed into Azure from VS Code.

> [!div class="nextstepaction"]
> [Download Azure Tools extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Create app service step  1](<./includes/nodejs-mongodb/create-app-service-vscode-01.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-01-240px.png" alt-text="A screenshot showing the location of the Azure Tools icon in the left toolbar." lightbox="./media/nodejs-mongodb/create-app-service-vscode-01.png"::: |
| [!INCLUDE [Create app service step  2](<./includes/nodejs-mongodb/create-app-service-vscode-02.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-02-240px.png" alt-text="A screenshot showing the App Service section of Azure Tools showing how to create a new web app." lightbox="./media/nodejs-mongodb/create-app-service-vscode-02.png"::: |
| [!INCLUDE [Create app service step  3](<./includes/nodejs-mongodb/create-app-service-vscode-03.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-03-240px.png" alt-text="A screenshot showing the dialog box used to enter the name of the web app in Azure." lightbox="./media/nodejs-mongodb/create-app-service-vscode-03.png"::: |
| [!INCLUDE [Create app service step  4](<./includes/nodejs-mongodb/create-app-service-vscode-04.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-04-240px.png" alt-text="A screenshot of dialog box used to select a resource group or create a new one for the web app." lightbox="./media/nodejs-mongodb/create-app-service-vscode-04.png"::: |
| [!INCLUDE [Create app service step  5](<./includes/nodejs-mongodb/create-app-service-vscode-05.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-05-240px.png" alt-text="A screenshot of the dialog box in VS Code used enter a name for the resource group." lightbox="./media/nodejs-mongodb/create-app-service-vscode-05.png"::: |
| [!INCLUDE [Create app service step  6](<./includes/nodejs-mongodb/create-app-service-vscode-06.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-06-240px.png" alt-text="A screenshot of the dialog box in VS Code used to select Node 14 LTS as the runtime for the web app." lightbox="./media/nodejs-mongodb/create-app-service-vscode-06.png"::: |
| [!INCLUDE [Create app service step  7](<./includes/nodejs-mongodb/create-app-service-vscode-07.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-07-240px.png" alt-text="A screenshot of the dialog in VS Code used to select operating system to use for hosting the web app." lightbox="./media/nodejs-mongodb/create-app-service-vscode-07.png"::: |
| [!INCLUDE [Create app service step  8](<./includes/nodejs-mongodb/create-app-service-vscode-08.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-08-240px.png" alt-text="A screenshot of the dialog in VS Code used to select location of the web app resources." lightbox="./media/nodejs-mongodb/create-app-service-vscode-08.png"::: |
| [!INCLUDE [Create app service step  9](<./includes/nodejs-mongodb/create-app-service-vscode-09.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-09-240px.png" alt-text="A screenshot of the dialog in VS Code used to select an App Service plan or create a new one." lightbox="./media/nodejs-mongodb/create-app-service-vscode-09.png"::: |
| [!INCLUDE [Create app service step 10](<./includes/nodejs-mongodb/create-app-service-vscode-10.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-10-240px.png" alt-text="A screenshot of the dialog in VS Code used to enter the name of the App Service plan." lightbox="./media/nodejs-mongodb/create-app-service-vscode-10.png"::: |
| [!INCLUDE [Create app service step 11](<./includes/nodejs-mongodb/create-app-service-vscode-11.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-11-240px.png" alt-text="A screenshot of the dialog in VS Code used to select the pricing tier of the App Service plan." lightbox="./media/nodejs-mongodb/create-app-service-vscode-11.png"::: |
| [!INCLUDE [Create app service step 12](<./includes/nodejs-mongodb/create-app-service-vscode-12.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-app-service-vscode-12-240px.png" alt-text="A screenshot of the dialog in VS Code asking if you want to create an App Insights resource for your web app." lightbox="./media/nodejs-mongodb/create-app-service-vscode-12.png"::: |

### [Azure CLI](#tab/azure-cli)

Azure CLI commands can be run in the [Azure Cloud Shell](https://shell.azure.com) or on a workstation with the [Azure CLI installed](/cli/azure/install-azure-cli). You can view or the [complete Azure CLI script for creating Azure resources](https://github.com/Azure-Samples/msdocs-nodejs-mongodb-azure-sample-app/blob/main/scripts/create-nodejs-mongodb-resources.sh) in the GitHub repository for this tutorial.

First, create a resource group to act as a container for all of the Azure resources related to this application.

```azurecli
LOCATION='eastus'                          # Use 'az account list-locations --output table' to list locations
RESOURCE_GROUP_NAME='msdocs-expressjs-mondgodb-tutorial'

# Create a resource group
az group create \
    --location $LOCATION \
    --name $RESOURCE_GROUP_NAME
```

Next, create an App Service plan using the [az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create) command.

* The `--sku` parameter defines the size (CPU, memory) and cost of the app service plan.  This example uses the F1 (Free) service plan.  For a full list of App Service plans, view the [App Service pricing](https://azure.microsoft.com/pricing/details/app-service/windows/) page.
* The `--is-linux` flag selects the Linux as the host operating system.  To use Windows, remove this flag from the command.

```azurecli
APP_SERVICE_PLAN_NAME='msdocs-expressjs-mongodb-plan'    

az appservice plan create \
    --name $APP_SERVICE_PLAN_NAME \
    --resource-group $RESOURCE_GROUP_NAME \
    --sku B1 \
    --is-linux
```

Finally, create the App Service web app using the [az webapp create](/cli/azure/webapp#az_webapp_create) command.  

* The *app service name* is used as both the name of the resource in Azure and to form the fully qualified domain name for your app in the form of `https://<app service name>.azurewebsites.com`.
* The runtime specifies what version of Node your app is running. This example uses Node 14 LTS. To list all available runtimes, use the command `az webapp list-runtimes --linux --output table` for Linux and `az webapp list-runtimes --output table` for Windows.

```azurecli
APP_SERVICE_NAME='msdocs-expressjs-mongodb-123'     # Change 123 to any three characters to form a unique name across Azure

az webapp create \
    --name $APP_SERVICE_NAME \
    --runtime 'NODE|14-lts'
    --plan $APP_SERVICE_PLAN_NAME
    --resource-group $RESOURCE_GROUP_NAME 
```

---

## 2 - Create an Azure Cosmos DB in MongoDB compatibility mode

Azure Cosmos DB is a fully managed NoSQL database for modern app development. Among its features is a 100% MongoDB compatible API allowing you to use your existing MongoDB tools, packages, and applications with Cosmos DB.

### [Azure portal](#tab/azure-portal)

You must be signed in to the [Azure portal](https://portal.azure.com/) to complete these steps to create a Cosmos DB.

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Create Cosmos DB step 1](<./includes/nodejs-mongodb/create-cosmos-db-azure-portal-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-azure-portal-1-240px.png" alt-text="A screenshot showing how to use the search box in the top tool bar to find Cosmos DB in Azure." lightbox="./media/nodejs-mongodb/create-cosmos-db-azure-portal-1.png"::: |
| [!INCLUDE [Create Cosmos DB step 2](<./includes/nodejs-mongodb/create-cosmos-db-azure-portal-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-azure-portal-2-240px.png" alt-text="A screenshot showing the create button on the Cosmos DB page used to create a database." lightbox="./media/nodejs-mongodb/create-cosmos-db-azure-portal-2.png"::: |
| [!INCLUDE [Create Cosmos DB step 3](<./includes/nodejs-mongodb/create-cosmos-db-azure-portal-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-azure-portal-3-240px.png" alt-text="A screenshot showing the page where you select the MongoDB API for your Cosmos DB." lightbox="./media/nodejs-mongodb/create-cosmos-db-azure-portal-3.png"::: |
| [!INCLUDE [Create Cosmos DB step 4](<./includes/nodejs-mongodb/create-cosmos-db-azure-portal-4.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-azure-portal-4-240px.png" alt-text="A screenshot showing how to fill out the page to create a new Cosmos DB." lightbox="./media/nodejs-mongodb/create-cosmos-db-azure-portal-4.png"::: |

### [VS Code](#tab/vscode-aztools)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Create Cosmos DB step 1](<./includes/nodejs-mongodb/create-cosmos-db-vscode-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-1-240px.png" alt-text="A screenshot showing the databases component of the Azure Tools VS Code extension and the location of the button to create a new database." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-1.png"::: |
| [!INCLUDE [Create Cosmos DB step 2](<./includes/nodejs-mongodb/create-cosmos-db-vscode-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-2-240px.png" alt-text="A screenshot showing the dialog box used to select the subscription for the new database in Azure." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-2.png"::: |
| [!INCLUDE [Create Cosmos DB step 3](<./includes/nodejs-mongodb/create-cosmos-db-vscode-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-3-240px.png" alt-text="A screenshot showing the dialog box used to select the type of database you want to create in Azure." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-3.png"::: |
| [!INCLUDE [Create Cosmos DB step 4](<./includes/nodejs-mongodb/create-cosmos-db-vscode-4.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-4-240px.png" alt-text="A screenshot of dialog box used to enter the name of the new database in Visual Studio Code." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-4.png"::: |
| [!INCLUDE [Create Cosmos DB step 5](<./includes/nodejs-mongodb/create-cosmos-db-vscode-5.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-5-240px.png" alt-text="A screenshot of the dialog to select the throughput mode of the database." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-5.png"::: |
| [!INCLUDE [Create Cosmos DB step 6](<./includes/nodejs-mongodb/create-cosmos-db-vscode-6.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-6-240px.png" alt-text="A screenshot of the dialog in VS Code used to select resource group to put the new database in." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-6.png"::: |
| [!INCLUDE [Create Cosmos DB step 7](<./includes/nodejs-mongodb/create-cosmos-db-vscode-7.md>)] | :::image type="content" source="./media/nodejs-mongodb/create-cosmos-db-vscode-7-240px.png" alt-text="A screenshot of the dialog in VS Code used to select location for the new database." lightbox="./media/nodejs-mongodb/create-cosmos-db-vscode-7.png"::: |

### [Azure CLI](#tab/azure-cli)

A new Azure Cosmos DB account is created by using the [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) command.

* The name of the Cosmos DB account must be unique across Azure. The name can only contain lowercase letters, numbers, and the hyphen (-) character and must be between 3 and 50 characters long.
* The `--kind MongoDB` flag tells Azure to create a Cosmos DB that is compatible with the MongoDB API.  This flag must be included for your Cosmos DB to work as a MongoDB database.

```azurecli
COSMOS_DB_NAME='msdocs-expressjs-mongodb-database-123'   # Replace 123 with any three characters to form a unique name

az cosmosdb create \
    --name $COSMOS_DB_NAME \
    --resource-group $RESOURCE_GROUP_NAME \
    --kind MongoDB
```

Creating a new Azure Cosmos DB typically takes about 5 minutes.

---

## 3 - Connect your App Service to your Cosmos DB

To connect to your Cosmos DB database, you need to provide the connection string for the database to your application. This is done in the sample application by reading the `DATABASE_URL` environment variable. When running locally, the sample application uses the [dotenv package](https://www.npmjs.com/package/dotenv) to read the connection string value from the `.env` file.

When running in Azure, configuration values like connection strings can be stored in the *application settings* of the App Service hosting the web app. These values are then made available to your application as environment variables during runtime. In this way, the application accesses the connection string from `process.env` the same way whether being run locally or in Azure. Further, this eliminates the need to manage and deploy environment specific config files with your application.

### [Azure portal](#tab/azure-portal)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Connection string step 1](<./includes/nodejs-mongodb/connection-string-azure-portal-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-azure-portal-1-240px.png" alt-text="A screenshot showing the location of the Cosmos DB connection string on the Cosmos DB quick start page." lightbox="./media/nodejs-mongodb/connection-string-azure-portal-1.png"::: |
| [!INCLUDE [Connection string step 2](<./includes/nodejs-mongodb/connection-string-azure-portal-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-azure-portal-2-240px.png" alt-text="A screenshot showing how to search for and navigate to the App Service where the connection string needs to store the connection string." lightbox="./media/nodejs-mongodb/connection-string-azure-portal-2.png"::: |
| [!INCLUDE [Connection string step 3](<./includes/nodejs-mongodb/connection-string-azure-portal-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-azure-portal-3-240px.png" alt-text="A screenshot showing how to access the Application settings within an App Service." lightbox="./media/nodejs-mongodb/connection-string-azure-portal-3.png"::: |
| [!INCLUDE [Connection string step 4](<./includes/nodejs-mongodb/connection-string-azure-portal-4.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-azure-portal-4-240px.png" alt-text="A screenshot showing the dialog used to set an application setting in Azure App Service." lightbox="./media/nodejs-mongodb/connection-string-azure-portal-4.png"::: |

### [VS Code](#tab/vscode-aztools)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Connection string step 1](<./includes/nodejs-mongodb/connection-string-vscode-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-vscode-1-240px.png" alt-text="A screenshot showing how to copy the connection string for a Cosmos database to your clipboard in VS Code." lightbox="./media/nodejs-mongodb/connection-string-vscode-1.png"::: |
| [!INCLUDE [Connection string step 2](<./includes/nodejs-mongodb/connection-string-vscode-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-vscode-2-240px.png" alt-text="A screenshot showing how to add a config setting to an App Service in VS Code." lightbox="./media/nodejs-mongodb/connection-string-vscode-2.png"::: |
| [!INCLUDE [Connection string step 3](<./includes/nodejs-mongodb/connection-string-vscode-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-vscode-3-240px.png" alt-text="A screenshot showing the dialog box used to give a name to an app setting in VS Code." lightbox="./media/nodejs-mongodb/connection-string-vscode-3.png"::: |
| [!INCLUDE [Connection string step 4](<./includes/nodejs-mongodb/connection-string-vscode-4.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-vscode-4-240px.png" alt-text="A screenshot showing the dialog used to set the value of an app setting in VS Code." lightbox="./media/nodejs-mongodb/connection-string-vscode-4.png"::: |
| [!INCLUDE [Connection string step 4](<./includes/nodejs-mongodb/connection-string-vscode-5.md>)] | :::image type="content" source="./media/nodejs-mongodb/connection-string-vscode-5-240px.png" alt-text="A screenshot showing how to view an app setting for an App Service in VS Code." lightbox="./media/nodejs-mongodb/connection-string-vscode-5.png"::: |

### [Azure CLI](#tab/azure-cli)

To get the connection string for a Cosmos DB database, use the [az cosmos keys list](/cli/azure/cosmosdb/keys) command.

```azurecli
az cosmosdb keys list \
    --type connection-strings \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $COSMOS_DB_NAME \
    --query "connectionStrings[?description=='Primary MongoDB Connection String'].connectionString" \
    --output tsv
```

Rather then copying and pasting the value, the connection string can be stored in a variable to make the next step easier.

```azurecli
COSMOS_DB_CONNECTION_STRING=`az cosmosdb keys list \
    --type connection-strings \
    --resource-group $RESOURCE_GROUP_NAME \
    --name $COSMOS_DB_NAME \
    --query "connectionStrings[?description=='Primary MongoDB Connection String'].connectionString" \
    --output tsv`
```

The [az webapp config appsettings](/cli/azure/webapp/config/appsettings) command is used to set application setting values for an App Service web app.  One or more key-value pairs are set using the `--settings` parameter. To set the `DATABASE_URL` value to the connection string for your web app, use the following command.

```azurecli
az webapp config appsettings set \
    --name $APP_SERVICE_NAME \
    --resource-group $RESOURCE_GROUP_NAME \
    --settings DATABASE_URL=$COSMOS_DB_CONNECTION_STRING
```

---

## 4 - Deploy application code to Azure

Azure App service supports multiple methods to deploy your application code to Azure including support for GitHub Actions and all major CI/CD tools. This article focuses on how to deploy your code from your local workstation to Azure.

### [Deploy using VS Code](#tab/vscode-deploy)

To deploy your application code directly from VS Code, you must have the [Azure Tools extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) installed and be signed into Azure from VS Code.

> [!div class="nextstepaction"]
> [Download Azure Tools extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Deploy from VS Code 1](<./includes/nodejs-mongodb/deploy-vscode-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/deploy-vscode-1-240px.png" alt-text="A screenshot showing the location of the Azure Tool icon in Visual Studio Code." lightbox="./media/nodejs-mongodb/deploy-vscode-1.png"::: |
| [!INCLUDE [Deploy from VS Code 2](<./includes/nodejs-mongodb/deploy-vscode-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/deploy-vscode-2-240px.png" alt-text="A screenshot showing how you deploy an application to Azure by right-clicking on a web app in VS Code and selecting deploy from the context menu." lightbox="./media/nodejs-mongodb/deploy-vscode-2.png"::: |
| [!INCLUDE [Deploy from VS Code 3](<./includes/nodejs-mongodb/deploy-vscode-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/deploy-vscode-3-240px.png" alt-text="A screenshot showing the Output window of VS Code while deploying an application to Azure." lightbox="./media/nodejs-mongodb/deploy-vscode-3.png"::: |

### [Deploy using Local Git](#tab/local-git-deploy)

[!INCLUDE [Deploy using Local Git](<./includes/nodejs-mongodb/deploy-local-git.md>)]

### [Deploy using a ZIP file](#tab/azure-cli-deploy)

[!INCLUDE [Deploy ZIP](<./includes/nodejs-mongodb/deploy-zip-file.md>)]

---

## 5 - Browse to the application

The application will have a url of the form `https://<app name>.azurewebsites.net`. Browse to this URL to view the application.

Use the form elements in the application to add and complete tasks.

![A screenshot showing the application running in a browser.](./media/nodejs-mongodb/sample-app-in-browser.png)

## 6 - Configure and view application logs

Azure App Service captures all messages logged to the console to assist you in diagnosing issues with your application. The sample app outputs console log messages in each of its endpoints to demonstrate this capability.  For example, the `get` endpoint outputs a message about the number of tasks retrieved from the database and an error message if something goes wrong.

:::code language="javascript" source="~/../msdocs-nodejs-mongodb-azure-sample-app/src/routes/index.js" range="7-21" highlight="8,12":::

The contents of the App Service diagnostic logs can be reviewed in the Azure portal, VS Code, or using the Azure CLI.

### [Azure portal](#tab/azure-portal)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Stream logs from Azure portal 1](<./includes/nodejs-mongodb/stream-logs-azure-portal-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/stream-logs-azure-portal-1-240px.png" alt-text="A screenshot showing the location of the Azure Tool icon in Visual Studio Code." lightbox="./media/nodejs-mongodb/stream-logs-azure-portal-1.png"::: |
| [!INCLUDE [Stream logs from Azure portal 2](<./includes/nodejs-mongodb/stream-logs-azure-portal-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/stream-logs-azure-portal-2-240px.png" alt-text="A screenshot showing how you deploy an application to Azure by right-clicking on a web app in VS Code and selecting deploy from the context menu." lightbox="./media/nodejs-mongodb//stream-logs-azure-portal-2.png"::: |

### [VS Code](#tab/vscode-aztools)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Stream logs from VS Code 1](<./includes/nodejs-mongodb/stream-logs-vscode-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/stream-logs-vscode-1-240px.png" alt-text="A screenshot showing the location of the Azure Tool icon in Visual Studio Code." lightbox="./media/nodejs-mongodb/stream-logs-vscode-1.png"::: |
| [!INCLUDE [Stream logs from VS Code 2](<./includes/nodejs-mongodb/stream-logs-vscode-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/stream-logs-vscode-2-240px.png" alt-text="A screenshot showing how you deploy an application to Azure by right-clicking on a web app in VS Code and selecting deploy from the context menu." lightbox="./media/nodejs-mongodb/stream-logs-vscode-2.png"::: |

### [Azure CLI](#tab/azure-cli)

First, you need to configure Azure App Service to output logs to the App Service filesystem using the [az webapp log config](/cli/azure/webapp/log#az_webapp_log_config) command.

```azurecli
az webapp log config \
    --web-server-logging 'filesystem' \
    --name $APP_SERVICE_NAME \
    --resource-group $RESOURCE_GROUP_NAME
```

To stream logs, use the [az webapp log tail](/cli/azure/webapp/log#az_webapp_log_tail) command.

```azurecli
az webapp log tail \
    --name $APP_SERVICE_NAME \
    --resource-group $RESOURCE_GROUP_NAME
```

Refresh the home page in the app or attempt other requests to generate some log messages. The output should look similar to the following.

```Console
2021-10-26T20:12:01.825485319Z npm start
2021-10-26T20:12:04.478474807Z npm info it worked if it ends with ok
2021-10-26T20:12:04.496736134Z npm info using npm@6.14.10
2021-10-26T20:12:04.497958909Z npm info using node@v14.15.1
2021-10-26T20:12:05.874225522Z npm info lifecycle todolist@0.0.0~prestart: todolist@0.0.0
2021-10-26T20:12:05.891572192Z npm info lifecycle todolist@0.0.0~start: todolist@0.0.0
2021-10-26T20:12:05.941127150Z
2021-10-26T20:12:05.941161452Z > todolist@0.0.0 start /home/site/wwwroot
2021-10-26T20:12:05.941168852Z > node ./bin/www
2021-10-26T20:12:05.941173652Z
2021-10-26T20:12:16.234642191Z Mongoose connection open to database
2021-10-26T20:12:19.360371481Z GET /robots933456.txt 404 2144.146 ms - 1497

2021-10-26T20:12:38.419182028Z Total tasks: 6   Current tasks: 3    Completed tasks:  3
2021-10-26T20:12:38.799957538Z GET / 304 500.485 ms - -
2021-10-26T20:12:38.900597945Z GET /stylesheets/style.css 304 2.574 ms - -
2021-10-26T20:12:38.900637447Z GET /css/bootstrap.css 304 12.300 ms - -
2021-10-26T20:12:38.903103684Z GET /images/Azure-A-48px-product.svg 304 8.896 ms - -
2021-10-26T20:12:38.904441659Z GET /js/bootstrap.min.js 304 9.372 ms - -
```

---

## 7 - Inspect deployed files using Kudu

Azure App Service provides a web-based diagnostics console named [Kudu](/azure/app-service/resources-kudu) that allows you to examine the server hosting environment for your web app. Using Kudu, you can view the files deployed to Azure, review the deployment history of the application, and even open an SSH session into the hosting environment.

To access Kudu, navigate to one of the following URLs. You will need to sign into the Kudu site with your Azure credentials.

* For apps deployed in Free, Shared, Basic, Standard, and Premium App Service plans - `https://<app-name>.scm.azurewebsites.net`
* For apps deployed in Isolated service plans - `https://<app-name>.scm.<ase-name>.p.azurewebsites.net`

From the main page in Kudu, you can access information about the application hosting environment, app settings, deployments, and browse the files in the wwwroot directory.

![A screenshot of the main page in the Kudu SCM app showing the different information available about the hosting environment.](./media/nodejs-mongodb/kudu-main-page.png)

Selecting the *Deployments* link under the REST API header will show you a history of deployments of your web app.

![A screenshot of the deployments JSON in the Kudu SCM app showing the history of deployments to this web app.](./media/nodejs-mongodb/kudu-deployments-list.png)

Selecting the *Site wwwroot* link under the Browse Directory heading allows you to browse and view the files on the web server.

![A screenshot of files in the wwwroot directory showing how Kudu allows you to see what has been deployed to Azure.](./media/nodejs-mongodb/kudu-wwwroot-files.png)

## Clean up resources

When you are finished, you can delete all of the resources from Azure by deleting the resource group for the application.

### [Azure portal](#tab/azure-portal)

Follow these steps while signed-in to the Azure portal to delete a resource group.

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Remove resource group Azure portal 1](<./includes/nodejs-mongodb/remove-resource-group-azure-portal-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/remove-resource-group-azure-portal-1-240px.png" alt-text="A screenshot showing how to search for and navigate to a resource group in the Azure portal." lightbox="./media/nodejs-mongodb/remove-resource-group-azure-portal-1.png"::: |
| [!INCLUDE [Remove resource group Azure portal 2](<./includes/nodejs-mongodb/remove-resource-group-azure-portal-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/remove-resource-group-azure-portal-2-240px.png" alt-text="A screenshot showing the location of the Delete Resource Group button in the Azure portal." lightbox="./media/nodejs-mongodb/remove-resource-group-azure-portal-2.png"::: |
| [!INCLUDE [Remove resource group Azure portal 3](<./includes/nodejs-mongodb/remove-resource-group-azure-portal-3.md>)] | :::image type="content" source="./media/nodejs-mongodb/remove-resource-group-azure-portal-3-240px.png" alt-text="A screenshot of the confirmation dialog for deleting a resource group in the Azure portal." lightbox="./media/nodejs-mongodb/remove-resource-group-azure-portal-3.png"::: |

### [VS Code](#tab/vscode-aztools)

| Instructions    | Screenshot |
|:----------------|-----------:|
| [!INCLUDE [Remove resource group VS Code 1](<./includes/nodejs-mongodb/remove-resource-group-vscode-1.md>)] | :::image type="content" source="./media/nodejs-mongodb/remove-resource-group-vscode-1-240px.png" alt-text="A screenshot showing how to delete a resource group in VS Code using the Azure Tools extention." lightbox="./media/nodejs-mongodb/remove-resource-group-vscode-1.png"::: |
| [!INCLUDE [Remove resource group VS Code 2](<./includes/nodejs-mongodb/remove-resource-group-vscode-2.md>)] | :::image type="content" source="./media/nodejs-mongodb/remove-resource-group-vscode-2-240px.png" alt-text="A screenshot of the confirmation dialog for deleting a resource group from VS Code." lightbox="./media/nodejs-mongodb/remove-resource-group-vscode-2.png"::: |

### [Azure CLI](#tab/azure-cli)

Delete the resource group by using the [az group delete](/cli/azure/group#az_group_delete) command.

:::code language="azurecli" source="~/../msdocs-nodejs-mongodb-azure-sample-app/scripts/delete-resource-group.sh":::

---

## Next steps

* Managed identity
* Performance monitoring
* User authentication
* CI/CD Setup

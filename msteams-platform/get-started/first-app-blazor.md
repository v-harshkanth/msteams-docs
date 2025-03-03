---
title: Get started - Build your first Teams app with Blazor
author: adrianhall
description: Quickly create a Microsoft Teams app that displays a "Hello, World!" message using the Microsoft Teams Toolkit and .NET Blazor.
ms.author: adhal
ms.date: 04/27/2021
ms.topic: quickstart
ms.localizationpriority: none
---

# Build and run your first Microsoft Teams app with Blazor

In this tutorial, you will learn how to create a new Microsoft Teams app in .NET/Blazor that implements a simple personal app to pull information from the Microsoft Graph. For example, a *personal app* includes a set of tabs for individual use. During the tutorial, you will learn about the structure of a Teams app, how to run an app locally, and how to deploy the app to Azure.

The app that is built displays basic user information for the current user.  When permission is granted, the app will connect to the Microsoft Graph as the current user to get the complete profile.

## Before you begin

Make sure your development environment is set up by installing the prerequisites.

> [!div class="nextstepaction"]
> [Install prerequisites](prerequisites.md)

## Create your project

Use the Teams Toolkit to create your first project:

1. Open Visual Studio 2019.

1. Select **Create a new project**.

1. Select **Microsoft Teams App**, then select **Next**.  To help you find the template, use the project type **Microsoft Teams**.

1. Enter a name and select **Next**.

1. Enter the application name and company name.

1. Select **Create**.  The application name and company name are displayed to your end users. Your Teams app will be created within a few seconds.  After the project is created, set up single sign-on with M365:

   1. Select **Project** > **TeamsFx** > **Configure for SSO...**.
   1. When prompted, sign in to your M365 administrator account.

---

## Take a tour of the source code

If you wish to skip this section for now, you can [run your app locally](#run-your-app-locally).

After the Teams Toolkit configures your project, you have the components to build a basic personal app for Teams. The project directories and files display in the Solution Explorer area of Visual Studio 2019.

:::image type="content" source="../assets/images/teams-toolkit-v2/blazor-file-layout.png" alt-text="Screenshot showing app project files for a personal app in Visual Studio 2019.":::

- The app icons are stored as PNG files in `color.png` and `outline.png`.
- The app manifest for publishing through the Developer Portal for Teams is stored in `Properties/manifest.json`.
- A backend controller is provided in `Controllers/BackendController.cs` for assisting with authentication.

Since you created a tab app during setup, the Teams Toolkit scaffolds all the necessary code for a basic tab as a [Blazor Server](/aspnet/core/blazor).

- `Pages/Tab.razor` is the front-end application's entry point.
- `TeamsFx.cs` and `JS/src/index.js` is used for initializing communications with the Teams host.

You can add backend functionality by adding additional ASP.NET Core controllers to your application.

## Run your app locally

Teams Toolkit allows you to run your app locally.  This consists of several parts that are necessary to provide the correct infrastructure that Teams expects:

- An application is registered with Azure Active Directory.  This application has permissions associated with the location that the app is loaded from and any backend resources it accesses.
- A web API is hosted (via IIS Express) to assist with authentication tasks, acting as a proxy between the app and Azure Active Directory.  
- An app manifest is generated and exists in the Developer Portal for Teams.  Teams uses the app manifest to tell connected clients where to load the app from.

After this is done, the app can be loaded within the Teams client.  We use the Teams web client so that we can see the HTML, CSS, and JavaScript code within a standard web development environment.

To build and run your app locally:


1. From Visual Studio Code, press the **F5** key to run your application in debug mode.


1. If requested, install the self-signed SSL certificate for local debugging.

   :::image type="content" source="../assets/images/teams-toolkit-v2/ssl-prompt.png" alt-text="Screenshot showing how the prompt to install a SSL certificate to enable Teams to load your application from localhost.":::

1. Teams will be loaded in a web browser, and you will be prompted to sign in. If prompted to open Microsoft Teams, select Cancel to remain within the browser. Sign in with your M365 account.

1. When prompted to install the app onto Teams, select **Add**.

   Your app will now be displayed:

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-completed-app.png" alt-text="Screenshot of the completed app":::

   You can perform the debugging activities as if this were any other web application like setting breakpoints. The app supports hot reloading.  If you change any file within the project, the page will be reloaded.

<!-- markdownlint-disable MD033 -->
<details>
<summary>Learn what happens when you run your app locally in the debugger.</summary>

When you press the **F5** key, the Teams Toolkit:

1. Registers your application with Azure Active Directory.
1. Registers your application for "side loading" in Microsoft Teams.
1. Starts your application backend running locally.
1. Starts your application front-end hosted locally.
1. Starts Microsoft Teams in a web browser with a command to instruct Teams to side load the application (the URL is registered inside the application manifest).

</details>

<!-- markdownlint-disable MD033 -->
<details>
<summary>Learn how to troubleshoot common issues when running your app locally.</summary>

To successfully run your app in Teams, you must have a Microsoft 365 development account that allows app side loading. For more information on account opening, see [Prerequisites](prerequisites.md#enable-sideloading).

</details>

## Deploy your app to Azure

Deployment consists of two steps: 

1. Necessary cloud resources are created. This is also known as provisioning.
1. Start coding and copy your app into the created cloud resources.

> **PREVIEW**
>
> Support for Blazor apps is new in Teams Toolkit.  Provisioning and deployment are done with a combination of Visual Studio 2019 and the Developer Portal for Teams.

## Provision and deploy your app to Azure App Service

1. In Solution Explorer, right-click the project node and select **Publish**. You can also use the **Build** > **Publish** menu item.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish1.png" alt-text="Select the Publish operation on the project":::

1. In the **Publish** window, select **Azure** and select **Next**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish2.png" alt-text="Select Azure as the publishing target":::

1. Select **Azure App Service (Windows)** and select **Next**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish3.png" alt-text="Select Azure App Service as the publishing target":::

1. Select **+** to create a new App Service instance.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish4.png" alt-text="Create a new instance.":::

1. In the **Create App Service (Windows)** dialog, the **Name**, **Subscription name**, **Resource Group**, and **Hosting Plan** entry fields are populated. If you have already got an App Service running, existing settings are selected.  You can opt to create a new resource group and hosting plan.  When ready, select **Create**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish5.png" alt-text="Select hosting plan and subscription":::

1. In the **Publish** dialog, the newly created instance has been automatically selected.  When ready, select **Finish**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish6.png" alt-text="Select the new instance.":::

1. Select the **Edit** (pencil) icon next to **Deployment Mode**, and select **Self-contained**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish8.png" alt-text="Select self-contained deployment mode.":::

1. Select **Publish**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/blazor-vs2019-publish7.png" alt-text="Publish your app to app service":::

   Visual Studio deploys the app to your Azure App Service, and the web app loads in your browser.  Add `/tab` to the end of the URL to see your page.

   The project properties **Publish** pane shows the site URL and other details. Make a note of the site URL.

## Create an environment for your app

The Developer Portal for Teams manages where the tabs for your app are loaded from with an **Environment**.  

**To create an environment:**

1. Open the [Developer Portal for Teams](https://dev.teams.microsoft.com).  Sign in with your M365 administrative account.

1. From the sidebar, select **Apps**.

1. If you only have one app, it will be automatically selected.  If not, select your app from the list.

1. Select **Environments**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments1.png" alt-text="Select environments":::

1. Select **Create your first environment**.

1. Enter a name for your environment and select **Add**. For example, `_Production_`.

1. Select **Create your first environment variable**.

1. Enter `azure_app_url` for the **Name**.  Enter your Azure site URL without the `https://` as the **Value**.

    :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments2.png" alt-text="Create environment variable":::

   Press **Add**.

## Update the app manifest

The app manifest loads the tab from a `localhost` URL.  In this section, you will configure the app manifest to load the tab from the URL listed within the environment you just created.

1. From the sidebar, select **Basic information**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments3.png" alt-text="Select basic information":::

1. There are several places within the manifest that list a `localhost:XXXXX` as part of a URL.  Replace all occurrences with `{{azure_app_url}}`, including the curly braces.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments4.png" alt-text="Adjust basic information for the environment":::

1. When complete, select **Save**.

1. From the sidebar, select **Capabilities**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments5.png" alt-text="Select capabilities":::

1. Select **Personal Tab**.
1. Next to the **Personal Tab**, select the triple dots, then select **Edit**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments6.png" alt-text="Edit personal tab settings":::

1. Replace the URL with the environment variable within the **Content Url** and **Website Url** fields.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments7.png" alt-text="Edit personal tab URLs":::

1. Select **Update**.

1. Select **Save**.

1. From the sidebar, select **Single Sign-On**.

1. Replace the `localhost` within the **Application ID URI** with `{{azure_app_url}}`.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments8.png" alt-text="Edit single sign-on Application ID URI":::

1. Select **Save**.

1. From the sidebar, select **Domains**.

1. Select **Add a domain**.

1. If `{{azure_app_url}}` is not listed as a valid domain, add it as a valid domain, then select **Add**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/devcenter-environments9.png" alt-text="Add a domain":::

   You can now use the **Preview in Teams** option at the top of the page to launch your app within Teams.

## See also

* [Tutorials Overview](code-samples.md)
* [Create a conversational bot app](first-app-bot.md)
* [Create a messaging extension](first-message-extension.md)
* [Code Samples](https://github.com/OfficeDev/Microsoft-Teams-Samples)

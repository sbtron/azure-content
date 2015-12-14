<properties
	pageTitle="Authentication and authorization for API Apps in Azure App Service | Microsoft Azure"
	description="Learn about the authentication and authorization services that Azure App Service provides for API Apps."
	services="app-service\api"
	documentationCenter=".net"
	authors="tdykstra"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-api"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="12/04/2015"
	ms.author="tdykstra"/>

# Authentication and authorization for API Apps in Azure App Service

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

## Overview 

This article describes the options for handling authentication and authorization for API Apps in Azure App Service. 

The following diagram illustrates some key characteristics of App Service authentication:

* It preprocesses incoming API requests, giving you several options for how much authentication work you want to do in your own code. 
* It supports five authentication providers:  Azure Active Directory, Facebook, Google, Twitter, and Microsoft Account.
* It works for both end user and service principal authentication. 
* It works the same for API Apps, Web Apps, and Mobile Apps.

![](./media/app-service-api-authentication/api-apps-overview.png)

## Preprocessing incoming requests

App Service can prevent anonymous HTTP requests from reaching your API app, or authenticate those that have tokens before they reach your API app. For an API app you can configure one of three options:

1. Allow only authenticated requests to reach your API app.

	If an anonymous request is received from a browser, App Service will redirect to a logon page. 

	That works if you know in advance which authentication provider (Google, Twitter, etc.) you want to use, you can configure App Service to handle the logon process for you.  As an alternative, you can specify your own URL to which App Service will redirect anonymous requests. You can then give users a choice of authentication providers.

	With this option, you don't need to write any authentication code at all in your app, and authorization is simplified because the most important claims are provided in the HTTP headers.

2. Allow all requests to reach your API app, but validate authenticated requests and pass along authentication information in the HTTP headers.

	This option gives you more flexibility in handling anonymous requests, and makes it easy to write code that needs access to the most common claims. Unlike option 1, you have to write code if you want to prevent anonymous users from using your API. 

3. Allow all requests to reach your API, take no action on authentication information in the requests.

	This option leaves the tasks of authentication and authorization entirely up to your application code.

In the [Azure portal](https://portal.azure.com/), you select the option you want on the **Authentication / Authorization** blade.

![](./media/app-service-api-authentication/authblade.png)

For options 1 and 2, turn on **App Service Authentication**, and in the **Action to take when requests is not authenticated** drop-down list choose **Log in** or **Allow request (no action)**.  If you choose **Log in**, you have to choose an authentication provider and configure that provider.

![](./media/app-service-api-authentication/actiontotake.png)
 
## Language agnostic

App Service authentication processing happens before requests reach your API app, which means that the authentication features work for API apps written in any language or framework.  Your API can be based on ASP.NET, Java, Node.js, or any framework that App Service supports.

App Service passes on the JWT token in the Authorization header of an HTTP request, and code written in any language or framework can get the information it needs from the token. In addition, App Service gives you easier access to the most commonly used claims by setting some special headers, such as the following:

* X-MS-CLIENT-PRINCIPAL-NAME
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
In a .NET API, you can use the `Authorize` attribute, and for fine-grained authorization you can easily write code based on claims because claims information is populated for you in .NET classes.

## <a id="internal"></a> Service account authentication

You can also use App Service authentication for internal scenarios such as for calling from one API app to another API app. In this scenario you can use credentials for a service account instead of end user credentials for authentication. A service account is also known as a *service principal* in Azure Active Directory, and authentication using such an account is also known as a service-to-service scenario. 

For service-to-service scenarios, you can protect the called API app by using Azure Active Directory, and provide an AAD service principal authorization token when you call the API app. You can request the token by providing the client ID and client secret from the AAD application. No special Azure-only code is required, such as used to be true for handling the Mobile Services Zumo token. An example of this scenario using ASP.NET API apps is covered by the tutorial [Service principal authentication for API Apps](app-service-api-dotnet-service-principal-auth.md).

If you want to handle a service-to-service scenario without using App Service authentication, you can use client certificates or basic authentication. For information about client certificates in Azure, see [How To Configure TLS Mutual Authentication for Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

Service account authentication from an App Service logic app to an API app is a special case that is explained in [Using your custom API hosted on App Service with Logic apps](../app-service-logic/app-service-logic-custom-hosted-api.md).

## More information

For more information about authentication and authorization services in Azure App Service, see [Expanding App Service authentication / authorization](/blog/announcing-app-service-authentication-authorization/).

## Next steps

This article has explained authentication and authorization features of App Service API apps. 

If you are following the getting started sequence of tutorials for ASP.NET and API Apps, try out these features in the next tutorial, [user authentication in App Service API Apps](app-service-api-dotnet-user-principal-auth.md).

For more information about using Node and Java in Azure App Service, see the [Node.js Developer Center](/develop/nodejs/) and the [Java Developer Center](/develop/java/).

# Lab-04 - OAuth2.0 authentication and authorization

## Introduction to OAuth2.0


## Overall architecture explain

![Architecure](./assets/lab04-architecture.png)

Users will `authenticate` with `Azure AD` through Web application or SPA (Single Page application), The Azure API Management (APIM) will not involve in this process (authentication). However, once the authentication is done, SPA will have the accessToken in JWT format, which contains various claims.

The `APIM` with proper policy configured will be able to use those `claims` to do the `authorization` process and make decision whether or not it should forward the request to upstream service

Upstream (or backend services) can be running in any runtimes, e.g. `Azure App Service`, `AKS`, or just simple `Virtual Machine`

#### Consideration
1. With this architecture, we can offload `Authentication/Authorization` out of the application, However, it's important to protect the upstream/backend service in the network layer. Which mean there is only one way to access those services which is through the APIM

2. In real-world, always consider to add another layer of security in front of `Azure APIM`, it's recommended to place the Web Application Firewall (WAF) such as, `Azure Frontdoor` in front of `Azure API Management`, so that it can help to protect against other threats, such as DDos, etc.


## Exercise 1 - Configure Azure AD
- Login to Azure Portal and navigate to `Azure Active Drirectory`
- Click `App Registration` on the menu

![App Registration](./assets/lab04-ex1-app-registration01.png)

- Click `+ Add` button

![App Registration](./assets/lab04-ex1-app-registration02.png)

- Enter application name, for example `workshop-web-app`
- Select `Accounts in this organization directory only`
- In `Redirect URI`, select platform to `Single-page application(SPA)` and redirect uri to `http://localhost:3000/`
- Then click `Register`
- When your application successfully registered, copy the `Application (Client) ID`, this value will be used to configure your SPA in `Exercise 2`

![App Registration](./assets/lab04-ex01-app-registration03.png)

- Next we will add API permission to our SPA app, by click `API permissions` on the menu
- Click `Add permission`, Select `Microsoft Graph`, then select `delegated permissions`
- Select `offline_access` and `openid` permissions, then click `Add permissions`

![API Permissions](./assets/lab04-ex01-app-permission01.png)

- Get Tenant ID from the `Overview` page of your `Azure AD`, this value will be used to configuring SPA in `Exercise 2`

![Tanent ID](./assets/lab04-ex1-tanent-id.png)

- You're set!!!

## Exercise 2 - Setup frontend application (SPA)
Zeabix has prepare simple React Application (SPA) for this workshop, let's configure it to authenticate with your Azure AD

- Clone the GitHub repository https://github.com/zeabix-cloud-native/workshop-web-app.git

- Open and Edit file `src/authConfig.js`
```js
auth: {
        clientId: '<your Application (Client) ID', // This is the ONLY mandatory field that you need to supply.
        authority: 'https://login.microsoftonline.com/<your Tenant ID>/', // Replace the placeholder with your tenant subdomain 
        redirectUri: '/', // Points to window.location.origin. You must register this URI on Azure Portal/App Registration.
        postLogoutRedirectUri: '/', // Indicates the page to navigate after logout.
        navigateToLoginRequestUrl: false, // If "true", will navigate back to the original request location before processing the auth code response.
    },
```

- Update `clientId` to your `Application (client) ID`
- Update `authority`, replace `<your Tenant ID>` with your tenant ID
- Run command `npm install` to download dependencies libraries
- Start Application by run command `npm start`
- You can access this SPA using any web browser, e.g. chrome and navigate to `http://localhost:3000`


## Exercise 3 - Examine the Token from Azure AD

## Exercise 4 - Implement APIM Policy to Validate JWT Token

## Exercise 5 - Testing 
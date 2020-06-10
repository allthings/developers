# Quickstart Guide

This guide will walk you through the process of creating a _very_ basic client-side integration and can be used as the basis for developing simple Micro-Apps on the Allthings Platform.

In this guide, you will learn how to:

1. [Obtain access](#obtaining-access)
1. [Create a simple integration](#a-simple-integration)


## Obtaining Access

All authorization on the Allthings Platform is done with [OAuth 2.0](https://oauth.net/2/). As a developer, you'll need to create an Allthings OAuth Client in the [Developer Console](https://console.allthings.me/projects) to gain access to APIs.

### Creating an OAuth Client

To create an OAuth Client, head over to the [Developer Console](https://console.allthings.me/projects). If you don't yet have an Allthings user account, you can create one when prompted to log in by following the "Sign Up" link.

Next, once logged in to the Developer Console, follow these steps to create a new OAuth Client:

1. From the "My Projects" overview screen, click on "New project" or click [here](https://console.allthings.me/projects/create) to create a new project directly. When prompted, enter a project name and description to complete the project creation. You can change these fields later.
1. Once your project is created, you'll be taken to the new project's details screen. You can also get back to your new project's details screen from the "My Projects" overview by clicking on "View" on your project. On your project's details screen, using the left-hand menu "Permissions" section, navigate to the "OAuth clients" screen.
1. From your project's "OAuth clients" screen, click on "Create OAuth Client" to create a new OAuth Client for your project.

![Creating an OAuth Code Flow public client](https://raw.githubusercontent.com/allthings/developers/master/guides/assets/guides.quickstart.creating-an-oauth-client.1.png)

Allthings allows developers to make use of three different types of OAuth Clients: Code Flow ([public-client authorization-code flow](https://oauth.net/2/grant-types/authorization-code/)), Code Flow ([confidential-client authorization-code flow](https://oauth.net/2/grant-types/authorization-code/)), and System-to-System ([client-credentials flow](https://oauth.net/2/grant-types/client-credentials/)). System-to-system clients require special permissions be granted to them. In order to use a system-to-system client in production, please [contact our support team](https://support.allthings.me/hc/en-us/requests/new).

> If you wish to perform API requests as, or on behalf of a user, you will want to create a _Code Flow_ client. If you want to make machine-to-machine API requests, for example as part of a Webhook-based integration, you'll want to create a _System-to-System_ client. You can find further details about OAuth and OAuth Clients in our Authorization documentation [here](../oauth.md).

For the purposes of this Quickstart guide, let's create a "Code Flow - client only" client. This type of client (a [public client](https://oauth.net/2/client-types/)) is intended for applications running in a browser or on a mobile device.

![Configuring the OAuth client](https://raw.githubusercontent.com/allthings/developers/master/guides/assets/guides.quickstart.creating-an-oauth-client.2.png)

Once you've created your OAuth client, you'll be presented with a screen that allows you to configure a number of fields on the OAuth Client:

- **Client ID**: This is the ID of your OAuth Client. You need to provide it during the OAuth flow. We'll use it later in this guide.
- **Name**: The name of your OAuth Client. This name is displayed to users when they perform the authorization flow with your Client.
- **Redirect URIs**: A Redirect URI is a URI (or URL) where a user will be redirected after authorizing your application. Your OAuth Client can have more than one redirect URI configured but you are required to use one of these during the OAuth authorization flow. We'll touch on it again later in this guide.
- **Terms of use** & **Privacy Policy** fields: In production, you must provide at least one of these. These are shown to users on the Authorization screen during the OAuth Code Flow. For this guickstart guide, you can use a test value like [https://example.test/](https://example.test/).

With an OAuth client created, we're ready to use it. In the next section we'll create a simple integration.


## A simple integration

Our simple integration will be, well, _very_ simple. We will ask a user to authorize our integration so that we can perform API requests as, or on behalf of that user. Then, in our simple web application we will display a welcome message with the user's name. That's it. While simple, this is the basis on which you'll be able to build further features simply by making further API requests or integrating with your own APIs. Let's get started.

> You can jump to the complete example source code [here](#complete-example).
> You can also run the completed example in your browser [here](https://codesandbox.io/s/the-smallest-allthings-micro-app-v2-biod3?fontsize=14&hidenavigation=1&theme=dark). Just make sure to update the _clientId_ with your OAuth Client ID and update your OAuth Client with your _Redirect URL_.

We'll use [CodeSandbox](https://codesandbox.io/) in this guide as it allows us to build this whole integration without ever needing to install anything locally on your computer or to run your own server—and it doesn't require any signup.

Let's create a new sandbox. Open up [this link](https://codesandbox.io/s/github/codesandbox-app/static-template/tree/master/?file=/index.html) in a new browser tab or window. You'll be presented with a small code editor on the left of your screen, and a preview on the right.

Delete all of the text in the _index.html_ file the editor window. Next, copy the following code and paste it into _index.html_.


```js
<html>
  <head>
    <title>Allthings Quickstart Micro-App</title>
    <meta charset="UTF-8" />
    <script src="https://unpkg.com/@allthings/sdk@latest/dist/lib.umd.min.js"></script>
  </head>

  <body>
    <div id="container">Authorizing...</div>
    
    <script type="text/javascript">
      window.addEventListener('load', async () => {
        // we'll add code here.
      })
    </script>    
  </body>
</html>    
```

This markup code lays down some scaffolding. Of interest is line 5. There we load the [Allthings Javascript SDK](https://github.com/allthings/node-sdk). The SDK will do most of the heavy lifting for us including abstracting away most of the complexity of the OAuth authorization flow.

Having pasted the above code into CodeSandbox and saving your changes to _index.html_, you should see something like this:

![Example index.html on the left, browser preview on the right](https://raw.githubusercontent.com/allthings/developers/master/guides/assets/guides.quickstart.a-simple-integration.1.png)

Next, replace the text `// we'll add code here.` with the following code:

```js
const currentUrl = new URL(window.location.href)
const currentUrlClean = window.location.href.split('?')[0]

const maybeAuthorizationCode = currentUrl.searchParams.get('code')
const maybeOauthState = currentUrl.searchParams.get('state')

const client = allthings.restClient({
  authorizationCode: maybeAuthorizationCode,
  clientId:
    '👉 Put your OAuth Client ID here',
  redirectUri: currentUrlClean,
})

const localOauthState = localStorage.getItem('oauthState')

if (!maybeAuthorizationCode || localOauthState !== maybeOauthState) {
  const state = client.oauth.generateState()
  const authorizationUri = client.oauth.authorizationCode.getUri(state)

  localStorage.setItem('oauthState', state)

  return (window.location = authorizationUri)
}

localStorage.removeItem('oauthState')
window.history.replaceState(undefined, undefined, currentUrlClean)

// Copy the following line, too! We'll add more code here later on.
// We'll make an API request here.

```

The above code is all you will need to make authorization with OAuth work.
In the code you just pasted, we need to replace `👉 Put your OAuth Client ID here` with your Oauth Client's _Client ID_. You can find your OAuth Client's _Client ID_ back in the [Developer Console](https://console.allthings.me/projects) where you previously created your OAuth Client.

```diff
const client = allthings.restClient({
  authorizationCode: maybeAuthorizationCode,
  clientId:
-    '👉 Put your OAuth Client ID here',
+    'example397d6d9260e2d955d_example2daoc5oogwkpkksw484aacwcw84404cc8o48ccko84',
  redirectUri: currentUrlClean,
})
```

Next, we need to configure the  _RedirectUri_ in your OAuth Client. Again, in the Developer Console, on the same screen you've copied the _Client ID_ from, we need to add a new redirect URI. Add your CodeSandbox's preview URL as a new redirect URI to your OAuth Client. You'll find your CodeSandbox's preview URL in the left preview pane.

When adding a redirect URI, make sure to click the "add" button, then "save".

Also make sure that you've also provided either a Terms of Use or Privacy Policy link (or both) on the OAuth Client. Without one of these, you won't be able to authorize users.

![Example index.html on the left, browser preview on the right](https://raw.githubusercontent.com/allthings/developers/master/guides/assets/guides.quickstart.a-simple-integration.2.gif)

With all that setup, let's add the final piece of code. Replace the line `// We'll make an API request here.` with the following code:

```js
const user = await client.getCurrentUser()

document.getElementById('container').innerHTML = `<p>Hi there, ${
  user.username
} (${user.email}). You're logged in!</p>`
```

These few lines will fetch information about the currently logged in user from the Allthings API and display it in the page. We make use of the Javascript SDKs `client.getCurrentUser()` method to do all of the work for us.

Now, make sure to save your changes to _index.html_ in CodeSandbox.

Finally, we can test our simple integration. Open the preview URL you copied earlier (the _redirectUri_) in a new browser tab or window. If everything was set up correctly, you will be redirected to the Allthings ID account service and prompted to authorize your integration (OAuth Client). Once you've granted authorization, you'll be redirected to your _redirectUri_, in this case, your CodeSandbox URL where you'll be greeted with a welcome message, `Hi there, {{username}}. You're logged in!`

🎉 That's it! You've just completed your first Allthings integration!


### Complete Example

[![Edit The smallest Allthings micro-app (v2)](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/the-smallest-allthings-micro-app-v2-biod3?fontsize=14&hidenavigation=1&theme=dark)

```js
<html>
  <head>
    <title>Allthings Quickstart Micro-App</title>
    <meta charset="UTF-8" />
    <script src="https://unpkg.com/@allthings/sdk@latest/dist/lib.umd.min.js"></script>
  </head>

  <body>
    <div id="container">Authorizing...</div>

    <script type="text/javascript">
      window.addEventListener('load', async () => {
        const currentUrl = new URL(window.location.href)
        const currentUrlClean = window.location.href.split('?')[0]

        const maybeAuthorizationCode = currentUrl.searchParams.get('code')
        const maybeOauthState = currentUrl.searchParams.get('state')

        const client = allthings.restClient({
          authorizationCode: maybeAuthorizationCode,
          clientId:
            '👉 Put your OAuth Client ID here',
          redirectUri: currentUrlClean,
        })

        const localOauthState = localStorage.getItem('oauthState')

        if (!maybeAuthorizationCode || localOauthState !== maybeOauthState) {
          const state = client.oauth.generateState()
          const authorizationUri = client.oauth.authorizationCode.getUri(state)

          localStorage.setItem('oauthState', state)

          return (window.location = authorizationUri)
        }

        localStorage.removeItem('oauthState')
        window.history.replaceState(undefined, undefined, currentUrlClean)

        const user = await client.getCurrentUser()

        document.getElementById('container').innerHTML = `<p>Hi there, ${
          user.username
        } (${user.email}). You're logged in!</p>`
      })
    </script>
  </body>
</html>
```

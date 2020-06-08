# Quickstart

This guide will walk you through the process of creating a _very_ basic client-side integration and can be used as the basis for developing simple Micro-Apps on the Allthings Platform.

In this guide, you will learn how to:

1. [Obtain access](#obtaining-access)
1. [Create a simple integration](#a-simple-integration)


## Obtaining Access

All authorization on the Allthings Platform is done with [OAuth 2.0](https://oauth.net/2/). As a developer, you'll need to create your own OAuth Client to gain access.

### Creating an OAuth Client

To create an OAuth Client, head over to the [Developer Console](https://console.allthings.me/projects). If you don't yet have an Allthings user account, you can create one when prompted to log in by following the "Sign Up" link.

Once logged in to the Developer Console, [create a new project](https://console.allthings.me/projects/create). Enter a project name and description to complete the project creation. You can change these fields later.

Once your project is created, you'll be taken to the new project's details screen. Using the left-hand menu "Permissions" section, navigate to the "OAuth clients" screen. From this screen we can create new OAuth Clients.

![Creating an OAuth Code Flow public client](https://raw.githubusercontent.com/allthings/developers/master/guides/assets/guides.quickstart.creating-an-oauth-client.1.png)

Allthings allows developers to make use of three different types of OAuth Clients: Code Flow ([public-client authorization-code flow](https://oauth.net/2/grant-types/authorization-code/)), Code Flow ([confidential-client authorization-code flow](https://oauth.net/2/grant-types/authorization-code/)), and System-to-System ([client-credentials flow](https://oauth.net/2/grant-types/client-credentials/)). System-to-system clients require special permissions be granted to them. In order to use a system-to-system client in production, please [contact our support team](https://support.allthings.me/hc/en-us/requests/new).

> If you wish to perform API requests as, or on behalf of a user, you will want to create a _Code Flow_ client. If you want to make machine-to-machine API requests, for example as part of a Webhook-based integration, you'll want to create a _System-to-System_ client. You can find further details about OAuth and OAuth Clients in our Authorization documentation [here](../oauth.md).

For the purposes of this Quickstart guide, let's create a "Code Flow - client only" client. This type of client (a [public client](https://oauth.net/2/client-types/)) is intended for applications running in a browser or on a mobile device.

![Configuring the OAuth client](https://raw.githubusercontent.com/allthings/developers/master/guides/assets/guides.quickstart.creating-an-oauth-client.2.png)

Once you've created your OAuth client, you'll be presented with a screen that allows you to configure a number of fields on the OAuth Client:

- **Client ID**: This is the ID of your OAuth Client. You need to provide it during the OAuth flow. We'll use it later in this guide.
- **Redirect URIs**: A Redirect URI is a URI (or URL) where a user will be redirected after authorizing your application. Your OAuth Client can have more than one redirect URI configured but you are required to use one of these during the OAuth authorization flow. We'll touch on it again later in this guide.
- **Terms of use** & **Privacy Policy** fields: In production, you must provide at least one of these. These are shown to users on the Authorization screen during the OAuth Code Flow. For this guickstart guide, you can use a test value like [https://example.test/](https://example.test/).

With an OAuth client created, we're ready to use it. In the next section we'll create a simple integration.


## A simple integration


```html
<html>
  <head>
    <title>Allthings Quickstart Micro-App</title>
    <meta charset="UTF-8" />
    <script src="https://unpkg.com/@allthings/sdk@latest/dist/lib.umd.min.js"></script>
  </head>

  <body>
    <div id="container">Authorizing...</div>
  </body>
</html>    
```

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

# Quickstart

This guide will walk you through the process of creating a _very_ basic client-side integration and can be used as the basis for developing simple Micro-Apps on the Allthings Platform.

In this guide, you will learn how to:

1. [Obtain access](#obtaining-access)
1. [Create a simple integration](#creating-a-simple-integration)


## Obtaining Access

All authorization on the Allthings Platform is done with OAuth 2.0. As a developer, you'll need to create your own OAuth Client to gain access.

### Creating an OAuth Client

To create an OAuth Client, head over to the [Developer Console](https://console.allthings.me/projects). If you don't yet have an Allthings user account, you can create one when prompted to log in by following the "Sign Up" link.

Once logged in to the Developer Console, [create a new project](https://console.allthings.me/projects/create). Enter a project name and description to complete the project creation. You can change these fields later.

Once your project is created, you'll be taken to the new project's details screen. From here, using the left-hand menu "Permissions" section, navigate to the "OAuth clients" screen.

## A simple integration

```html
<html>
  <head>
    <title>Allthings Quickstart Micro-App</title>
    <meta charset="UTF-8" />
  </head>

  <body>
    <div id="container">Authorizing...</div>
  </body>
</html>    
```

```html
<html>
  <head>
    <title>Allthings Quickstart Micro-App</title>
    <meta charset="UTF-8" />
+    <script src="https://unpkg.com/@allthings/sdk@latest/dist/lib.umd.min.js"></script>
  </head>

  <body>
    <div id="container">Authorizing...</div>
  </body>
</html>    
```

```diff
<html>
  <head>
    <title>Allthings Quickstart Micro-App</title>
    <meta charset="UTF-8" />
+    <script src="https://unpkg.com/@allthings/sdk@latest/dist/lib.umd.min.js"></script>
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

```html
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

        document.getElementById('container').innerHTML = `<div>Hi there, ${
          user.username
        } (${user.email}). You're logged in!</div>`
      })
    </script>
  </body>
</html>
```

[![Edit The smallest Allthings micro-app (v2)](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/the-smallest-allthings-micro-app-v2-biod3?fontsize=14&hidenavigation=1&theme=dark)

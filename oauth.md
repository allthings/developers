
# OAuth 2.0 Integration

Detailed information regarding roles, protocol flow, authorization, endpoints
and tokens can be found at
[https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749) 

The following section explain how the ALLTHINGS platform uses the OAuth 2.0
protocol to authorize 3rd-party applications.

The OAuth protocol defines four roles which are adapted as follows:

OAuth                 | ALLTHINGS platform
----------------------|-------------------
Resource owner        | User
Resource server       | ALLTHINGS platform
Authorisation service | ALLTHINGS platform
Client application    | your Micro-App

The authorization the user grants to your app is represented by an
*authorization grant.*

OAuth defines four authorization grant types to
[obtain authorization](https://tools.ietf.org/html/rfc6749#section-4).

The ALLTHINGS platform supports two authorization grant types:

1. the **authorization code grant flow** and
2. the **implicit grant flow**.

**Notice**
> Please replace `api-sandbox.allthings.me` in the following sections with the
> domain of the ALLTHINGS app you want to authorize against.  
> Please also replace `www.example.com` with the domain of your
> [MicroApp](micro-app.md) or widget.

## Authorization Code Grant

Is to be used when a server acts as OAuth2 client and the client should have a
long term API access on behalf of the user. Invalid or expired access tokens can
be refreshed.

**This is the preferred authorization grant type for MicroApps.**

### 1. Authorization

When the user wants to use the 3rd-party application (i.e. the MicroApp), they
get redirected to the authorisation endpoint first:

`https://api-sandbox.allthings.me/auth/authorize`

The authorisation endpoint requires the following query parameters for the
authorization code grant:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by ALLTHINGS, `xxxxxx` in the example below
response_type | `code`
redirect_uri  | The MicroApp URL, e.g. `https://www.example.com`

Example URL:

`https://api-sandbox.allthings.me/auth/authorize?client_id=xxxxxx&response_type=code&redirect_uri=https%3A%2F%2Fwww.example.com`

As MicroApps usually run inside the context of the ALLTHINGS app, the user is
already authenticated at this point. If not, the user is redirected to a login
form.

After the user is authenticated, ALLTHINGS prompts the user to authorize your
MicroApp to access their data. The user can accept or decline your app and also
define the scope of data access for your MicroApp.

**Notice**
> If the user has already authorized your app, this step is skipped.  
> At the moment, only pre-authorized partner applications are allowed, so this
> step is always skipped.

### 2. Exchange auth code

When the authorization succeeded, the user gets redirected to the `redirect_uri`
with an auth code appended as `code` query parameter:

`https://www.example.com/?code=zzzzzz`

Your app will have to use the auth code to request an `access_token` by
sending a `POST` request to the token endpoint:

`https://app.allthings.me/auth/token`

**Notice**
> The token endpoint can be used on any app domain - e.g. the generic
> `app.allthings.me` in this example - as it does not require a user session.

The required `application/x-www-form-urlencoded` form data for the auth code
exchange:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by ALLTHINGS, `xxxxxx` in the example below
client_secret | Will be provided by ALLTHINGS, `yyyyyy` in the example below
grant_type    | `authorization_code`
code          | The auth code, `zzzzzz` in the example below
redirect_uri  | The MicroApp URL, e.g. `https://www.example.com`

Example request data:

`client_id=xxxxxx&client_secret=yyyyyy&grant_type=authorization_code&code=zzzzzz&redirect_uri=https%3A%2F%2Fwww.example.com`

The response contains an `access_token` and a `refresh_token`.  
The `access_token` can then be used to make api calls on behalf of the user,
while the `refresh_token` can be used to retrieve a new `access_token`.

Example response:

```json
{
  "access_token": "aaaaaa",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": null,
  "refresh_token": "bbbbbb",
  "auth_url": "https://api-sandbox.allthings.me/auth/authorize"
}
```

### 3. Make API calls

Your app can then use the `access_token` to authorize calls to the API by
sending it as `Authorization` HTTP header: 

```
Authorization: Bearer <access_token>
```

**Notice**
> `Bearer` is the token type defined in the JSON response.

### 4. Refreshing expired access token

When the `access_token` has expired (the lifetime is `3600` seconds) or becomes
invalid, you have to use the `refresh_token` to request a new `access_token` by
sending a `POST` request to the token endpoint:

`https://app.allthings.me/auth/token`

**Notice**
> The token endpoint can be used on any app domain - e.g. the generic
> `app.allthings.me` in this example - as it does not require a user session.

The required `application/x-www-form-urlencoded` form data to refresh the
`access_token`:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by ALLTHINGS, `xxxxxx` in the example below
client_secret | Will be provided by ALLTHINGS, `yyyyyy` in the example below
grant_type    | `refresh_token`
refresh_token | The refresh_token, `bbbbbb` in the example below

Example request data:

`client_id=xxxxxx&client_secret=yyyyyy&grant_type=refresh_token&refresh_token=bbbbbb`

The response contains an `access_token` and a `refresh_token`.  
The `access_token` can then be used to make api calls on behalf of the user,
while the `refresh_token` can be used to retrieve a new `access_token`.

Example response:

```json
{
  "access_token":  "aaaaaa",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": null,
  "refresh_token":  "bbbbbb",
  "auth_url": "https://api-sandbox.allthings.me/auth/authorize"
}
```

## Implicit Grant

Is to be used, when the OAuth client is implemented in a browser using a
scripting language such as JavaScript.

Since invalid or expired access tokens cannot be refreshed with the the implicit
grant type, this should only be used for widgets that are displayed for a
limited amount of time, e.g. one-off usage.

### 1. Authorization

When the user wants to use the 3rd-party application (i.e. a widget), they must
be redirected to the authorisation endpoint first:

`https://api-sandbox.allthings.me/auth/authorize`

The authorisation endpoint requires the following query parameters for the
implicit grant:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by ALLTHINGS, `xxxxxx` in the example below
response_type | `token`
redirect_uri  | The widget URL, e.g. `https://www.example.com`

Example URL:

`https://api-sandbox.allthings.me/auth/authorize?client_id=xxxxxx&response_type=token&redirect_uri=https%3A%2F%2Fwww.example.com`

If the user is not already logged into the app, they are redirected to a login
form.

After the user is authenticated, ALLTHINGS prompts the user to authorize your
widget to access their data. The user can accept or decline your app and also
define the scope of data access for your widget.

**Notice**
> If the user has already authorized your app, this step is skipped.  
> At the moment, only pre-authorized partner applications are allowed, so this
> step is always skipped.

When the authorization succeeded, the user gets redirected to the `redirect_uri`
with the access token as URI fragment:

`https://example.com#access_token=aaaaaa&expires_in=3600&token_type=bearer&auth_url=https%3A%2F%2Fapi-sandbox.allthings.me%2Fauth%2Fauthorize`

### 2. Make API calls

Your app can then use the `access_token` to authorize calls to the API by
sending it as `Authorization` HTTP header: 

```
Authorization: Bearer <access_token>
```

**Notice**
> `Bearer` is the token type defined in the URI fragment.

### 3. Expired access token

When the `access_token` expires, the whole widget has to be reloaded to restart
the authorization flow.

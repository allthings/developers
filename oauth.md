
# OAuth 2.0 Integration

Detailed information regarding roles, protocol flow, authorization, endpoints
and tokens can be found at
[https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749) 

The following section explain how the Allthings platform uses the OAuth 2.0
protocol to authorize 3rd-party applications.

The OAuth protocol defines four roles which are adapted as follows:

OAuth                 | Allthings platform
----------------------|-------------------
Resource owner        | User
Resource server       | Allthings platform
Authorisation service | Allthings accounts
Client application    | your Micro-App

The authorization the user grants to your app is represented by an
*authorization grant.*  
OAuth defines four authorization grant types to
[obtain authorization](https://tools.ietf.org/html/rfc6749#section-4).

The Allthings platform supports two authorization grant types:

1. The **implicit grant** flow *(Recommended if you are building a Webapp or Microapp)*
2. The **authorization code grant** flow

**Notice**
> Please replace `www.example.com` with the domain of your
> [MicroApp](guides/micro-app.md) or widget.

## URLs

### Authorization URL

`https://accounts.allthings.me/oauth/authorize`

### Token URL

`https://accounts.allthings.me/oauth/token`

## Implicit Grant

Is to be used, when the OAuth client is implemented in a browser using a
scripting language such as JavaScript. Choose this if you are building a Webapp or a Microapp.

Since invalid or expired access tokens cannot be refreshed with the the implicit
grant type, this should only be used for widgets that are displayed for a
limited amount of time, e.g. one-off usage.

### 1. Authorization

When the user wants to use the 3rd-party application (i.e. a widget), they must
be redirected to the authorisation endpoint first:

```
https://accounts.allthings.me/oauth/authorize
```

The authorisation endpoint requires the following query parameters for the
implicit grant:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by Allthings, `xxxxxx` in the example below
response_type | `token`
redirect_uri  | The widget URL, e.g. `https://www.example.com`
scope         | Specifies the scope of the access (currently only `user-profile:read`)
state         | Value used by the client to maintain state between the request and callback

**Example URL:**

```
https://accounts.allthings.me/oauth/authorize?client_id=xxxxxx&response_type=token&redirect_uri=https%3A%2F%2Fwww.example.com&scope=user-profile:read&state=XC13Did93
```

If the user is not already logged into the app, they are redirected to a login
form.

After the user is authenticated, Allthings prompts the user to authorize your
widget to access their data. The user can accept or decline your app and also
define the scope of data access for your widget.

**Notice**
> If the user has already authorized your app, this step is skipped.  

When the authorization succeeded, the user gets redirected to the `redirect_uri`
with the access token as URI fragment:

```
https://example.com#access_token=aaaaaa&expires_in=3600&token_type=bearer&state=XC13Did93
```

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

## Authorization Code Grant

Is to be used when a server acts as OAuth2 client and the client should have a
long term API access on behalf of the user. Invalid or expired access tokens can
be refreshed.

### 1. Authorization

When the user wants to use the 3rd-party application (i.e. the MicroApp), they
get redirected to the authorisation endpoint first:

```
https://accounts.allthings.me/oauth/authorize
```

The authorisation endpoint requires the following query parameters for the
authorization code grant:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by Allthings, `xxxxxx` in the example below
response_type | `code`
redirect_uri  | The MicroApp URL, e.g. `https://www.example.com`
scope         | Specifies the scope of the access (currently only `user-profile:read`)
state         | Value used by the client to maintain state between the request and callback

**Example URL:**

```
https://accounts.allthings.me/oauth/authorize?client_id=xxxxxx&response_type=code&redirect_uri=https%3A%2F%2Fwww.example.com&scope=user-profile:read
&state=XC13Did93
```

As MicroApps usually run inside the context of the Allthings app, the user is
already authenticated at this point. If not, the user is redirected to a login
form.

After the user is authenticated, Allthings prompts the user to authorize your
MicroApp to access their data. The user can accept or decline your app and also
define the scope of data access for your MicroApp.

**Notice**
> If the user has already authorized your app, this step is skipped.

### 2. Exchange auth code

When the authorization succeeded, the user gets redirected to the `redirect_uri`
with an auth code appended as `code` query parameter:

```
https://www.example.com/?code=zzzzzz&state=XC13Did93
```

Your app will have to use the auth code to request an `access_token` by
sending a `POST` request to the token endpoint:

```
https://accounts.allthings.me/oauth/token
```

The required form data for the auth code exchange:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by Allthings, `xxxxxx` in the example below
client_secret | Will be provided by Allthings, `yyyyyy` in the example below
grant_type    | `authorization_code`
code          | The auth code, `zzzzzz` in the example below
redirect_uri  | The MicroApp URL, e.g. `https://www.example.com`

**Notice**
> The form data fields need to be encoded as content type
`application/x-www-form-urlencoded`.

**Example request data:**

```
client_id=xxxxxx&client_secret=yyyyyy&grant_type=authorization_code&code=zzzzzz&redirect_uri=https%3A%2F%2Fwww.example.com
```

The response contains an `access_token` and a `refresh_token`.  
The `access_token` can then be used to make api calls on behalf of the user,
while the `refresh_token` can be used to retrieve a new `access_token`.

**Example response:**

```json
{
  "access_token": "aaaaaa",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": "user-profile:read",
  "refresh_token": "bbbbbb"
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

```
https://accounts.allthings.me/oauth/token
```

The required form data to refresh the `access_token`:

Key           | Value
--------------|----------------------------------------------------------------
client_id     | Will be provided by Allthings, `xxxxxx` in the example below
client_secret | Will be provided by Allthings, `yyyyyy` in the example below
grant_type    | `refresh_token`
refresh_token | The refresh_token, `bbbbbb` in the example below

**Notice**
> The form data fields need to be encoded as content type
`application/x-www-form-urlencoded`.

**Example request data:**

```
client_id=xxxxxx&client_secret=yyyyyy&grant_type=refresh_token&refresh_token=bbbbbb
```

The response contains an `access_token` and a `refresh_token`.  
The `access_token` can then be used to make api calls on behalf of the user,
while the `refresh_token` can be used to retrieve a new `access_token`.

**Example response:**

```json
{
  "access_token":  "aaaaaa",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": "user-profile:read",
  "refresh_token":  "bbbbbb"
}
```

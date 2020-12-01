# Micro-App

A Micro-App is a standalone web application embedded into the Allthings
interface.

## Without authorization

This is the simplest form of integration into the Allthings platform and only
requires a minimal amount of effort to ensure compatibility.

### Requirements

* [Responsive website](https://en.wikipedia.org/wiki/Responsive_web_design)
adapting to layouts from small smartphone screens to tablets and large desktop
computers.
* [HTTPS](https://en.wikipedia.org/wiki/HTTPS) connection for all resources.
* Interface design must follow our Micro-App [design guide](../design-guide/).

## With authorization

A Micro-App that needs to connect to user data or other data on the Allthings
platform has to be authorized via [OAuth 2.0](../oauth.md) and can therefore access
and modify resources on the Allthings platform on behalf of the user.

### Additional requirements

* Implementation of [OAuth 2.0](../oauth.md) authorization flow.

## Cookies

Cookies in 3rd-party Micro-Apps are by default **NOT** supported.
The way to keep a session must be purely via the OAuth flow (basically using 
the OAuth access token as session token).

In the case of integrating existing apps that cannot get rid of session cookies 
(e.g. shops), there’s one solution:
Running the MicroApp in a subdomain of the app domain (or on the same
Top-Level-Domain).

This is however not a scalable solution, as it involves manual adoptions
(setting up CNAME DNS entries and SSL certificates).

The reason Cookies are not supported is that Apple’s Safari rejects 3rd-party
cookies by default, no matter if they’re set in an iframe or via XHR request.
Safari will accept cookies from domains that have been visited before, but that
would only work if the Micro-App is opened in the top level frame.

## Security considerations

### Clickjacking mitigation

#### Modern browsers
To prevent [Clickjacking](https://en.wikipedia.org/wiki/Clickjacking), it is
recommended to set the
[Content-Security-Policy frame-ancestors](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
header, which is supported by all modern browsers.

Since MicroApps are displayed in an IFrame, the Allthings app domains need to be
allowed:
```
Content-Security-Policy: frame-ancestors *.allthings.me *.allthings.app;
```

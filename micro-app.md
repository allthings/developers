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
* Interface design must follow our Micro-App [design guide](design-guide/).

## With authorization

A Micro-App that needs to connect to user data or other data on the Allthings
platform has to be authorized via [OAuth 2.0](oauth.md) and can therefore access
and modify resources on the Allthings platform on behalf of the user.

### Additional requirements

* Implementation of [OAuth 2.0](oauth.md) authorization flow.

# Developer Guides

The Allthings platform has been built as an interoperable platform and provides
a [REST API](apis/core-data.md) to exchange data with third-party applications, as well as
a back-end for the Allthings app itself.

Third-party applications integrated into the Allthings interface are called
[Micro-Apps](guides/micro-app.md), which can be implemented either with or without user
authorization.

To provide a single-sign-on experience for the end user, Allthings makes use of
the open [OAuth 2.0](oauth.md) standard for third-party authorization.

All Micro-Apps should follow the [Design Guide](design-guide/) as good
as possible to retain a seamless look&feel across the whole Application. 

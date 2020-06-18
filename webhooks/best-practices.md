# Best practices

## Contents

1. [Protect data privacy](#)
1. [Verify events are sent from Allthings](#verify-events-are-sent-from-allthings)
   1. [Check signatures](#check-signatures)
   1. [Prevent replay attacks](#prevent-replay-attacks)
   1. [Example code](#example-code)


## Protect data privacy


## Verify events are sent from Allthings

### Check signatures

### Prevent replay attacks

### Example code

The following implementation of `hasVerifiedPayload()` checks both that the signature and the timestamp of the request are valid. This protects your webhook endpoint from both unauthorized 3rd-parties and from replay attacks.

```javascript
const crypto = require('crypto')

const TIMESTAMP_TOLERANCE_IN_MILISECONDS = 2 * 60 * 1000 // 2 minutes

exports.hasVerifiedPayload = ({ sharedSecret, headers, body }) => {
  const requestSecret = headers['x-allthings-signature']
  const requestTimestamp = Number(headers['x-allthings-signature-timestamp'])
  const now = Date.now()

  const signatureisOk =
    crypto
      .createHmac('sha256', sharedSecret)
      .update(body)
      .digest('hex') === requestSecret

  const timingIsOk =
    requestTimestamp < now &&
    requestTimestamp > now - TIMESTAMP_TOLERANCE_IN_MILISECONDS

  return signatureisOk && timingIsOk
}
```

Usage example:

```javascript
if (!hasVerifiedPayload({
  sharedSecret: process.env.SHARED_WEBHOOK_SECRET,
  headers: request.headers,
  body: request.body // make sure it's the raw request body string (not the parsed JSON)
})) {
  throw new Error('Invalid webhook signature.')
}
```

# Best practices

## Contents

1. [Protect users data privacy](#protect-users-data-privacy)
1. [Select only the event types that you need](#select-only-the-event-types-that-you-need)
1. [Delivery attempts and retries](#delivery-attempts-and-retries)
1. [Event handling](#event-handling)
1. [Verify events are sent from Allthings](#verify-events-are-sent-from-allthings)
   1. [Check signatures](#check-signatures)
   1. [Prevent replay attacks](#prevent-replay-attacks)
   1. [Example code](#example-code)


## Protect users data privacy

It is imperative that you treat data with respect and privacy in mind. In most situations, you or your legal entity will have entered into a Data Protection Agreement between Allthings and/or one of our mutual customers. You must treat data delivered to you responsibly. We reserve the right to deactivate webhooks without notice in cases where we suspect negligence.


## Select only the event types that you need

Listening for extra events (or all events) is not recommended. When configuring a webhook, you should only select those events which you need for your integration. Avoid subscribing to all events. There can be a lot of activity within an App, and by over-subscribing to events, your webhook endpoint may become overwhelmed.


## Delivery attempts and retries

We attempt to deliver events to your webhooks for up to seven days with an exponential back off. After seven days we will give up. You can still retrieve the events using the [Webhooks API](../apis/webhooks.md). You'll find more guidance on debugging or retrieving past delivery attempts [here](./build-webhooks.md#debugging-delivery-issues).


## Event handling

Though rare, your webhook endpoint might occasionally receive the same event more than once. You should guard against duplicated event receipts by making your event processing idempotent. As each event contains a unique identifier, idempotency can be achieved by storing a list of event IDs and only processing events with IDs which you haven't already processed.

The order of events is also not guaranteed. Do not expect Event A to always be delivered before Event B.


## Verify events are sent from Allthings

### Check signatures

Developers should verify the webhook payload signature to confirm that the data really came from Allthings.

This can be done by computing an HMAC with the SHA256 hash function. Use the endpoint’s shared signing secret as the key, and the raw request body sent by us to your webhook endpoint string as the message. Once computed, compare your hash against the hash that was sent in the `x-allthings-signature` header. If they are identical, then the request came from Allthings.


### Prevent replay attacks

To mitigate against [replay attacks](https://en.wikipedia.org/wiki/Replay_attack), we include a `x-allthings-signature-timestamp` header with each webhook delivery. Because this timestamp is part of the signed payload, it is also verified by the signature, so an attacker cannot change the timestamp without invalidating the signature. If the signature is valid but the timestamp is too old, you can have your application reject the payload.


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
  throw new Error('Cannot verify webhook.')
}
```

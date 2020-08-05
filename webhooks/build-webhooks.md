# Build a webhook endpoint

This page walks you through the code that properly handles webhook notifications.

To make use of Allthings webhooks, you first need to build your own custom endpoint. A webhook endpoint is no different from any other server-side functionality you might implement. At its simplest, it is some server-side code which can receive POST requests.

Before building a webhook endpoint, there are several key considerations regardless of the technology involved. Please also review the [best practices for using webhooks](./best-practices.md).


## Key considerations

For each event occurrence, Allthings POSTs the webhook data to your endpoint in JSON format. The full event details are included and can be used directly after parsing the JSON. Thus, at minimum, the webhook endpoint needs to expect data through a POST request and confirm successful receipt of that data.

When your endpoint receives an event, you must acknowledge receipt of the payload by returning a 2xx HTTP status code. All response codes outside this range, including 3xx codes, indicate to Allthings that you did not receive the event.

If your webhook endpoint does not return a 2xx HTTP status code, Allthings will attempt to deliver the event again. We will slowly back-off on attempted delivery until, after multiple failures to send the notification over multiple days, we mark the event as failed and stops trying to send it to your endpoint.

Because properly acknowledging receipt of the webhook notification is so important, your endpoint should return a 2xx HTTP status code prior to any complex logic could cause a timeout.

### Debugging delivery issues

As a developer, you can review all of the events Allthings attempted to deliver to your webhook endpoint using the Webhooks API.

Performing the following GraphQL query on the Webhooks API will return a list of all attempted webhook deliveries, responses from your endpoint, and the respective HTTP status codes we received. Delivery history is kept for 30 days after which time it is irreversibly deleted.


```graphql
query QueryWebhookDeliveries {
  webhook(id: "<your webhook ID here>") {
    id
    description
    url
    deliveries {
      totalCount
      nodes {
        id
        createdDate
        deliveryDate
        status
        result {
          body
          headers {
            name
            value
          }
          message
          statusCode
        }
        event {
          id
          name
          urn
          payload
          createdDate
        }
      }
    }
  }
}

```


## Example code

A complete example of a webhook endpoint which implements all of the best practices is available on CodeSandbox [here](https://codesandbox.io/s/allthings-webhook-example-m37vf?file=/readme.md).

[![Edit Allthings Webhook Example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/allthings-webhook-example-m37vf?file=/readme.md)

```js
const Koa = require('koa')
const bodyParser = require('koa-bodyparser')
const logger = require('koa-logger')
const Router = require('koa-router')

const PORT = process.env.PORT || 8000

const app = new Koa()
const router = new Router()

router.post('/webhook', async ctx => {
  const { headers, body } = ctx.request

  // If there is no signature in the headers,
  // we assume it's a test webhook delivery
  // and return status 200 immediately.
  if (!headers.hasOwnProperty('x-allthings-signature')) {
    ctx.status = 200
    ctx.body = {
      message: 'Received test event.',
    }

    return
  }

  // handle the event
  switch (body.event.name) {
    case 'ticket.created':
      const newTicket = body.event.data
      // do something with the new ticket
      break
    case 'ticket.updated':
      const changes = body.event.data.delta
      // do something with the update
      break
    default:
      ctx.status = 400
      ctx.body = {
        message: 'Unexpected event type',
      }
      
      return
  }

  ctx.status = 200
  ctx.body = {
    message: 'success',
  }
})

app
  .use(logger())
  .use(bodyParser())
  .use(router.routes())
  .use(router.allowedMethods())
  .listen(PORT, '0.0.0.0', () =>
    console.log(`listening on http://localhost:${PORT}...`)
  )
```




<br /><br /><br />
➡️ **Next Step:** [Read about best practices](./best-practices.md)

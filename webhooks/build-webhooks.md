# Build a webhook endpoint

This page walks you through the code that properly handles webhook notifications.

Before building a webhook endpoint, there are several key considerations regardless of the technology involved. Please also review the [best practices for using webhooks](#./best-practices.md).


## Key considerations


## Example code

A complete example of a webhook endpoint which implements all of the best practices is available on CodeSandbox [here](https://codesandbox.io/s/allthings-webhook-example-m37vf?file=/src/routes/webhook-post.js).

[![Edit Allthings Webhook Example](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/allthings-webhook-example-m37vf?file=/src/routes/webhook-post.js)

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

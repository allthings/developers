# Webhooks API

<details>
  <summary><strong>Table of Contents</strong></summary>

  * [Query](#query)
  * [Mutation](#mutation)
  * [Objects](#objects)
    * [Event](#event)
    * [HttpHeader](#httpheader)
    * [PageInfo](#pageinfo)
    * [RegisterableWebhookDescription](#registerablewebhookdescription)
    * [Webhook](#webhook)
    * [WebhookDeliveriesConnection](#webhookdeliveriesconnection)
    * [WebhookDelivery](#webhookdelivery)
    * [WebhookDeliveryEdge](#webhookdeliveryedge)
    * [WebhookDeliveryResult](#webhookdeliveryresult)
    * [WebhookEdge](#webhookedge)
    * [WebhookMutationResult](#webhookmutationresult)
    * [WebhooksConnection](#webhooksconnection)
  * [Inputs](#inputs)
    * [RegisterWebhookInput](#registerwebhookinput)
    * [UpdateWebhookInput](#updatewebhookinput)
    * [WebhooksFilter](#webhooksfilter)
  * [Enums](#enums)
    * [WebhookDeliveryStatus](#webhookdeliverystatus)
  * [Scalars](#scalars)
    * [Boolean](#boolean)
    * [DateTime](#datetime)
    * [EventName](#eventname)
    * [ID](#id)
    * [Int](#int)
    * [Json](#json)
    * [String](#string)
    * [Url](#url)
    * [Urn](#urn)
  * [Interfaces](#interfaces)
    * [MutationResult](#mutationresult)
    * [Node](#node)

</details>

## Query
Root query field

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>registerableWebhookEvents</strong></td>
<td valign="top">[<a href="#registerablewebhookdescription">RegisterableWebhookDescription</a>!]!</td>
<td>

Returns a list of events available for webhooks

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>webhook</strong></td>
<td valign="top"><a href="#webhook">Webhook</a></td>
<td>

Get a webhook by ID

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">id</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>webhooks</strong></td>
<td valign="top"><a href="#webhooksconnection">WebhooksConnection</a>!</td>
<td>

Get a list of all webhooks matching a filter.

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">filter</td>
<td valign="top"><a href="#webhooksfilter">WebhooksFilter</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">first</td>
<td valign="top"><a href="#int">Int</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">after</td>
<td valign="top"><a href="#id">ID</a></td>
<td></td>
</tr>
</tbody>
</table>

## Mutation
Root mutation field

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>registerWebhook</strong></td>
<td valign="top"><a href="#webhookmutationresult">WebhookMutationResult</a>!</td>
<td>

Creates and activates a new webhook

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">webhook</td>
<td valign="top"><a href="#registerwebhookinput">RegisterWebhookInput</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>updateWebhook</strong></td>
<td valign="top"><a href="#webhookmutationresult">WebhookMutationResult</a>!</td>
<td>

Update the configuration of an existing webhook

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">id</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">webhook</td>
<td valign="top"><a href="#updatewebhookinput">UpdateWebhookInput</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>removeWebhook</strong></td>
<td valign="top"><a href="#webhookmutationresult">WebhookMutationResult</a>!</td>
<td>

Deactivates and removes a webhook

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">id</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
</tbody>
</table>

## Objects

### Event

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>id</strong></td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>createdDate</strong></td>
<td valign="top"><a href="#datetime">DateTime</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>name</strong></td>
<td valign="top"><a href="#eventname">EventName</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>urn</strong></td>
<td valign="top"><a href="#urn">Urn</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>payload</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td>

Stringified JSON event payload

</td>
</tr>
</tbody>
</table>

### HttpHeader

HTTP Header

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>name</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>value</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
</tbody>
</table>

### PageInfo

Pagination information

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>endCursor</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

An end cursor for use in pagination.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>hasNextPage</strong></td>
<td valign="top"><a href="#boolean">Boolean</a>!</td>
<td>

Indicates if there are more pages to fetch.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>hasPreviousPage</strong></td>
<td valign="top"><a href="#boolean">Boolean</a>!</td>
<td>

Indicates if there are any pages prior to the current page.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>startCursor</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

An start cursor for use in pagination.

</td>
</tr>
</tbody>
</table>

### RegisterableWebhookDescription

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>namespace</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td>

Namespace of the event

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>eventName</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td>

The name of the event

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>description</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td>

A short name and/or description mostly for UI convenience

</td>
</tr>
</tbody>
</table>

### Webhook

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>id</strong></td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>createdDate</strong></td>
<td valign="top"><a href="#datetime">DateTime</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>events</strong></td>
<td valign="top">[<a href="#eventname">EventName</a>!]!</td>
<td>

Events to subscribe to

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>urns</strong></td>
<td valign="top">[<a href="#urn">Urn</a>!]!</td>
<td>

Event URNs to filter for

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>url</strong></td>
<td valign="top"><a href="#url">Url</a>!</td>
<td>

URL of HTTPS endpoint where webhook events are delivered to

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>enabled</strong></td>
<td valign="top"><a href="#boolean">Boolean</a>!</td>
<td>

Whether the webhook is enabled and receiving deliveries

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>deliveries</strong></td>
<td valign="top"><a href="#webhookdeliveriesconnection">WebhookDeliveriesConnection</a></td>
<td>

Get the history of all events delivered to the Webhook's URL

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">first</td>
<td valign="top"><a href="#int">Int</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">after</td>
<td valign="top"><a href="#id">ID</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>secret</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

A shared secret used to sign the request payload

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>description</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

A short name and/or description mostly for UI convenience

</td>
</tr>
</tbody>
</table>

### WebhookDeliveriesConnection

A Webhooks connection

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>edges</strong></td>
<td valign="top">[<a href="#webhookdeliveryedge">WebhookDeliveryEdge</a>!]!</td>
<td>

A list of node edges

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>nodes</strong></td>
<td valign="top">[<a href="#webhookdelivery">WebhookDelivery</a>!]!</td>
<td>

A list of nodes

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>pageInfo</strong></td>
<td valign="top"><a href="#pageinfo">PageInfo</a>!</td>
<td>

Information for pagination

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>totalCount</strong></td>
<td valign="top"><a href="#int">Int</a>!</td>
<td>

The total count of items in the connection.

</td>
</tr>
</tbody>
</table>

### WebhookDelivery

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>id</strong></td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>createdDate</strong></td>
<td valign="top"><a href="#datetime">DateTime</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>event</strong></td>
<td valign="top"><a href="#event">Event</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>deliveryDate</strong></td>
<td valign="top"><a href="#datetime">DateTime</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>status</strong></td>
<td valign="top"><a href="#webhookdeliverystatus">WebhookDeliveryStatus</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>retry</strong></td>
<td valign="top"><a href="#webhookdelivery">WebhookDelivery</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>result</strong></td>
<td valign="top"><a href="#webhookdeliveryresult">WebhookDeliveryResult</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>webhook</strong></td>
<td valign="top"><a href="#webhook">Webhook</a>!</td>
<td></td>
</tr>
</tbody>
</table>

### WebhookDeliveryEdge

A WebhookDelivery edge

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>cursor</strong></td>
<td valign="top"><a href="#id">ID</a>!</td>
<td>

A cursor for use in pagination.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>node</strong></td>
<td valign="top"><a href="#webhookdelivery">WebhookDelivery</a>!</td>
<td>

The item at the end of edge.

</td>
</tr>
</tbody>
</table>

### WebhookDeliveryResult

The HTTP Response from a Webhook Delivery attempt

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>body</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>headers</strong></td>
<td valign="top">[<a href="#httpheader">HttpHeader</a>]!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>message</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>statusCode</strong></td>
<td valign="top"><a href="#int">Int</a>!</td>
<td></td>
</tr>
</tbody>
</table>

### WebhookEdge

A Webhook edge

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>cursor</strong></td>
<td valign="top"><a href="#id">ID</a>!</td>
<td>

A cursor for use in pagination.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>node</strong></td>
<td valign="top"><a href="#webhook">Webhook</a>!</td>
<td>

The item at the end of edge.

</td>
</tr>
</tbody>
</table>

### WebhookMutationResult

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>code</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>success</strong></td>
<td valign="top"><a href="#boolean">Boolean</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>message</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>webhook</strong></td>
<td valign="top"><a href="#webhook">Webhook</a></td>
<td></td>
</tr>
</tbody>
</table>

### WebhooksConnection

A Webhooks connection

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>edges</strong></td>
<td valign="top">[<a href="#webhookedge">WebhookEdge</a>!]!</td>
<td>

A list of node edges

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>nodes</strong></td>
<td valign="top">[<a href="#webhook">Webhook</a>!]!</td>
<td>

A list of nodes

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>pageInfo</strong></td>
<td valign="top"><a href="#pageinfo">PageInfo</a>!</td>
<td>

Information for pagination

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>totalCount</strong></td>
<td valign="top"><a href="#int">Int</a>!</td>
<td>

The total count of items in the connection.

</td>
</tr>
</tbody>
</table>

## Inputs

### RegisterWebhookInput

<table>
<thead>
<tr>
<th colspan="2" align="left">Field</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>events</strong></td>
<td valign="top">[<a href="#eventname">EventName</a>!]!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>urns</strong></td>
<td valign="top">[<a href="#urn">Urn</a>!]!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>url</strong></td>
<td valign="top"><a href="#url">Url</a>!</td>
<td>

URL of HTTPS endpoint the webhook should deliver events to

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>enabled</strong></td>
<td valign="top"><a href="#boolean">Boolean</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>description</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
</tbody>
</table>

### UpdateWebhookInput

<table>
<thead>
<tr>
<th colspan="2" align="left">Field</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>events</strong></td>
<td valign="top">[<a href="#eventname">EventName</a>!]</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>urns</strong></td>
<td valign="top">[<a href="#urn">Urn</a>!]</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>url</strong></td>
<td valign="top"><a href="#url">Url</a></td>
<td>

URL of HTTPS endpoint the webhook should deliver events to

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>enabled</strong></td>
<td valign="top"><a href="#boolean">Boolean</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>description</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
</tbody>
</table>

### WebhooksFilter

<table>
<thead>
<tr>
<th colspan="2" align="left">Field</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>appId</strong></td>
<td valign="top"><a href="#id">ID</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>projectId</strong></td>
<td valign="top"><a href="#id">ID</a></td>
<td></td>
</tr>
</tbody>
</table>

## Enums

### WebhookDeliveryStatus

<table>
<thead>
<th align="left">Value</th>
<th align="left">Description</th>
</thead>
<tbody>
<tr>
<td valign="top"><strong>PENDING</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>SUCCESS</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>FAILED</strong></td>
<td></td>
</tr>
</tbody>
</table>

## Scalars

### Boolean

The `Boolean` scalar type represents `true` or `false`.

### DateTime

Datetime scalar type

### EventName

A string which contains only lowercase letters, dots and hyphens. For example: "community-article.published"

### ID

The `ID` scalar type represents a unique identifier, often used to refetch an object or as key for a cache. The ID type appears in a JSON response as a String; however, it is not intended to be human-readable. When expected as an input type, any string (such as `"4"`) or integer (such as `4`) input value will be accepted as an ID.

### Int

The `Int` scalar type represents non-fractional signed whole numeric values. Int can represent values between -(2^31) and 2^31 - 1.

### Json

### String

The `String` scalar type represents textual data, represented as UTF-8 character sequences. The String type is most often used by GraphQL to represent free-form human-readable text.

### Url

An https URL. For example: "https://example.allthings.me/webhook"

### Urn

A string which defines a urn. For example: "urn:allthings:app:82662ee4-0080-4960-8b8f-d7693e6ff3e4:*"


## Interfaces


### MutationResult

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>code</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>success</strong></td>
<td valign="top"><a href="#boolean">Boolean</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>message</strong></td>
<td valign="top"><a href="#string">String</a>!</td>
<td></td>
</tr>
</tbody>
</table>

### Node

An object with an ID to support global identification.

<table>
<thead>
<tr>
<th align="left">Field</th>
<th align="right">Argument</th>
<th align="left">Type</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="2" valign="top"><strong>id</strong></td>
<td valign="top"><a href="#id">ID</a>!</td>
<td>

The object's id

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>createdDate</strong></td>
<td valign="top"><a href="#datetime">DateTime</a>!</td>
<td>

Datetime when the object was created

</td>
</tr>
</tbody>
</table>

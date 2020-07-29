# DataAmp API

<details>
  <summary><strong>Table of Contents</strong></summary>

  * [Query](#query)
  * [Mutation](#mutation)
  * [Objects](#objects)
    * [AddressValue](#addressvalue)
    * [Item](#item)
    * [ItemConfiguration](#itemconfiguration)
    * [ItemEdge](#itemedge)
    * [ItemMutationResult](#itemmutationresult)
    * [ItemsConnection](#itemsconnection)
    * [JsonValue](#jsonvalue)
    * [OAuthClient](#oauthclient)
    * [PageInfo](#pageinfo)
    * [Parent](#parent)
    * [User](#user)
  * [Inputs](#inputs)
    * [ItemConfigurationInput](#itemconfigurationinput)
    * [ItemInput](#iteminput)
    * [ItemsFilter](#itemsfilter)
  * [Enums](#enums)
    * [ItemValueType](#itemvaluetype)
    * [ParentType](#parenttype)
  * [Scalars](#scalars)
    * [Boolean](#boolean)
    * [DateTime](#datetime)
    * [Float](#float)
    * [ID](#id)
    * [Int](#int)
    * [JSON](#json)
    * [KeyString](#keystring)
    * [String](#string)
    * [URN](#urn)
    * [UrnPattern](#urnpattern)
  * [Interfaces](#interfaces)
    * [MutationResult](#mutationresult)
    * [Node](#node)
    * [Value](#value)

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
<td colspan="2" valign="top"><strong>item</strong></td>
<td valign="top"><a href="#item">Item</a></td>
<td>

Retrieve a an item by its ID

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">id</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>uniqueItem</strong></td>
<td valign="top"><a href="#item">Item</a></td>
<td>

Retrieve a unique item by its parent and unique-key

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">parent</td>
<td valign="top"><a href="#urn">URN</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">key</td>
<td valign="top"><a href="#keystring">KeyString</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>items</strong></td>
<td valign="top"><a href="#itemsconnection">ItemsConnection</a>!</td>
<td>

Get all items on a parent-object URN matching an optional filter

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">parent</td>
<td valign="top"><a href="#urn">URN</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">filter</td>
<td valign="top"><a href="#itemsfilter">ItemsFilter</a></td>
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
<td colspan="2" valign="top"><strong>createItem</strong></td>
<td valign="top"><a href="#itemmutationresult">ItemMutationResult</a>!</td>
<td>

Create a new item

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">parent</td>
<td valign="top"><a href="#urn">URN</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">item</td>
<td valign="top"><a href="#iteminput">ItemInput</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>updateItemConfiguration</strong></td>
<td valign="top"><a href="#itemmutationresult">ItemMutationResult</a>!</td>
<td>

Update an existing item

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">item</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">options</td>
<td valign="top"><a href="#itemconfigurationinput">ItemConfigurationInput</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>setItemValue</strong></td>
<td valign="top"><a href="#itemmutationresult">ItemMutationResult</a>!</td>
<td>

Set an item's value

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">item</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">value</td>
<td valign="top"><a href="#json">JSON</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>deleteItem</strong></td>
<td valign="top"><a href="#itemmutationresult">ItemMutationResult</a>!</td>
<td>

Delete an item

</td>
</tr>
<tr>
<td colspan="2" align="right" valign="top">item</td>
<td valign="top"><a href="#id">ID</a>!</td>
<td></td>
</tr>
</tbody>
</table>

## Objects

### AddressValue

An item's value representing an address

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
<td colspan="2" valign="top"><strong>json</strong></td>
<td valign="top"><a href="#json">JSON</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>latitude</strong></td>
<td valign="top"><a href="#float">Float</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>longitude</strong></td>
<td valign="top"><a href="#float">Float</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>country</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>region</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

The region in which the address is located. For example: Canton  Zürich

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>locality</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

The city in which the address is located. For example: Zürich

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>street</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

The street on which the address is located. For example: Schaffhauserstrasse

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>addressLines</strong></td>
<td valign="top">[<a href="#string">String</a>]</td>
<td>

Address lines.

For example: ["434 Main Street"].

Address lines may hold components such as:
- Building
- Sub-Building
- Premise number (house number)
- Premise Range
- Thoroughfare
- Sub-Thoroughfare
- Double-Dependent Locality
- Sub-Locality

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>name</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

The name of the addressee. For example: Allthings Technologies AG

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>postalCode</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td>

The postal code corresponding to the address. For example: 8001

</td>
</tr>
</tbody>
</table>

### Item

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
<td>

Datetime when the item was created

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>createdBy</strong></td>
<td valign="top"><a href="#actor">Actor</a>!</td>
<td>

URN of the actor who created this item. E.g. urn:allthings:account:1234 or urn:allthings:oauth-client:1234

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>parent</strong></td>
<td valign="top"><a href="#parent">Parent</a>!</td>
<td>

The parent object this item is associated with

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>key</strong></td>
<td valign="top"><a href="#keystring">KeyString</a>!</td>
<td>

The key-name of the item.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>value</strong></td>
<td valign="top"><a href="#itemvalue">ItemValue</a></td>
<td>

The value or data of the item.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>configuration</strong></td>
<td valign="top"><a href="#itemconfiguration">ItemConfiguration</a>!</td>
<td></td>
</tr>
</tbody>
</table>

### ItemConfiguration

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
<td colspan="2" valign="top"><strong>userEditable</strong></td>
<td valign="top"><a href="#boolean">Boolean</a></td>
<td>

Can a user edit this item's value?

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>unique</strong></td>
<td valign="top"><a href="#boolean">Boolean</a></td>
<td>

Should the key be unique? cannot be changed to true if duplicates already exist. if true new items with same key cannot be created.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>expires</strong></td>
<td valign="top"><a href="#datetime">DateTime</a></td>
<td>

If set - a future date when this Item should be automatically purged. This will trigger an 'item.expired' event when the expiration occurs.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>valueType</strong></td>
<td valign="top"><a href="#itemvaluetype">ItemValueType</a></td>
<td>

Control the type of value when the item value is provided

</td>
</tr>
</tbody>
</table>

### ItemEdge

An Item edge

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
<td valign="top"><a href="#item">Item</a>!</td>
<td>

The item at the end of edge.

</td>
</tr>
</tbody>
</table>

### ItemMutationResult

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
<td colspan="2" valign="top"><strong>item</strong></td>
<td valign="top"><a href="#item">Item</a></td>
<td></td>
</tr>
</tbody>
</table>

### ItemsConnection

An Item connection

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
<td valign="top">[<a href="#itemedge">ItemEdge</a>!]!</td>
<td>

A list of node edges

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>nodes</strong></td>
<td valign="top">[<a href="#item">Item</a>!]!</td>
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

### JsonValue

An item's value representing any sort of JSON data.

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
<td colspan="2" valign="top"><strong>json</strong></td>
<td valign="top"><a href="#json">JSON</a></td>
<td></td>
</tr>
</tbody>
</table>

### OAuthClient

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
<td colspan="2" valign="top"><strong>urn</strong></td>
<td valign="top"><a href="#urn">URN</a>!</td>
<td>

The OAuthClient's URN

</td>
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

### Parent

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

The ID of the parent object (e.g. parent's foreign key)

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>type</strong></td>
<td valign="top"><a href="#parenttype">ParentType</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>urn</strong></td>
<td valign="top"><a href="#urn">URN</a>!</td>
<td></td>
</tr>
</tbody>
</table>

### User

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
<td colspan="2" valign="top"><strong>urn</strong></td>
<td valign="top"><a href="#urn">URN</a>!</td>
<td>

The user's URN

</td>
</tr>
</tbody>
</table>

## Inputs

### ItemConfigurationInput

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
<td colspan="2" valign="top"><strong>userEditable</strong></td>
<td valign="top"><a href="#boolean">Boolean</a></td>
<td>

Can a user edit this item's value?

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>unique</strong></td>
<td valign="top"><a href="#boolean">Boolean</a></td>
<td>

should the key be unique? cannot be changed to true if duplicates already exist. if true new items with same key cannot be created.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>expires</strong></td>
<td valign="top"><a href="#datetime">DateTime</a></td>
<td>

If set - a future date when this Item should be automatically purged. This will trigger an 'item.expired' event when the expiration occurs.

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>valueType</strong></td>
<td valign="top"><a href="#itemvaluetype">ItemValueType</a></td>
<td>

Control the type of value when the item value is provided

</td>
</tr>
</tbody>
</table>

### ItemInput

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
<td colspan="2" valign="top"><strong>key</strong></td>
<td valign="top"><a href="#keystring">KeyString</a>!</td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>value</strong></td>
<td valign="top"><a href="#json">JSON</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>configuration</strong></td>
<td valign="top"><a href="#itemconfigurationinput">ItemConfigurationInput</a></td>
<td></td>
</tr>
</tbody>
</table>

### ItemsFilter

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
<td colspan="2" valign="top"><strong>contains</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>keys</strong></td>
<td valign="top">[<a href="#id">ID</a>!]</td>
<td>

Only return items with these keys

Maximum keys: 100

</td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>keysStartWith</strong></td>
<td valign="top"><a href="#string">String</a></td>
<td></td>
</tr>
<tr>
<td colspan="2" valign="top"><strong>valueType</strong></td>
<td valign="top"><a href="#itemvaluetype">ItemValueType</a></td>
<td></td>
</tr>
</tbody>
</table>

## Enums

### ItemValueType

<table>
<thead>
<th align="left">Value</th>
<th align="left">Description</th>
</thead>
<tbody>
<tr>
<td valign="top"><strong>JsonValue</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>AddressValue</strong></td>
<td></td>
</tr>
</tbody>
</table>

### ParentType

<table>
<thead>
<th align="left">Value</th>
<th align="left">Description</th>
</thead>
<tbody>
<tr>
<td valign="top"><strong>account</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>app</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>property</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>group</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>unit</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>utilisationPeriod</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>ticket</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>booking</strong></td>
<td></td>
</tr>
<tr>
<td valign="top"><strong>serviceProvider</strong></td>
<td></td>
</tr>
</tbody>
</table>

## Scalars

### Boolean

The `Boolean` scalar type represents `true` or `false`.

### DateTime

Datetime scalar type

### Float

The `Float` scalar type represents signed double-precision fractional values as specified by [IEEE 754](https://en.wikipedia.org/wiki/IEEE_floating_point).

### ID

The `ID` scalar type represents a unique identifier, often used to refetch an object or as key for a cache. The ID type appears in a JSON response as a String; however, it is not intended to be human-readable. When expected as an input type, any string (such as `"4"`) or integer (such as `4`) input value will be accepted as an ID.

### Int

The `Int` scalar type represents non-fractional signed whole numeric values. Int can represent values between -(2^31) and 2^31 - 1.

### JSON

JSON scalar type

### KeyString

A string which matches /^([A-Za-z0-9-/])+$/gi. For example: "myItemKey"

### String

The `String` scalar type represents textual data, represented as UTF-8 character sequences. The String type is most often used by GraphQL to represent free-form human-readable text.

### URN

A string which defines a URN. For example: "urn:allthings:app:82662ee4-0080-4960-8b8f-d7693e6ff3e4"

### UrnPattern


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

### Value

An object that describes a item's value

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
<td colspan="2" valign="top"><strong>json</strong></td>
<td valign="top"><a href="#json">JSON</a></td>
<td></td>
</tr>
</tbody>
</table>

# ERP Data Exchange Specifications

This document specifies the Allthings ERP Data Exchange format.

## Contents

1.  [Import Process Outline](#import-process-outline)
1.  [CSV file specifications](#csv-file-specifications)
    1.  [uuidRemappings.csv](#uuidremappingscsv)
    1.  [properties.csv](#propertiescsv)
    1.  [groups.csv](#groupscsv)
    1.  [units.csv](#unitscsv)
    1.  [utilisationPeriods.csv](#utilisationperiodscsv)
    1.  [tenantCheckIns.csv](#tenantcheckinscsv)
    1.  [tenants.csv](#tenantscsv)
    1.  [agents.csv](#agentscsv)
    1.  [propertyTeams.csv](#propertyteamscsv)
    1.  [userRelations.csv](#userRelationscsv)
    1.  [manifest.json](#manifestjson)
1.  [Data Types](#data-types)
    1.  [Country Type](#country-type)
    1.  [Date Type](#date-type)
    1.  [Import Type Enumerable Type](#import-type)
    1.  [Phone Number Type](#phone-number-type)
    1.  [Resource Enumerable Type](#resource-type)
    1.  [String Type](#string-type)
    1.  [UUID Type](#uuid-type)

## Import Process Outline

Customer Support will provide a data upload location (an AWS S3 Bucket).
Uploading a set of CSVs and a [_manifest.json_](#manifestjson) file constitutes an _Import Job_.
The `manifest.json` file acts as a trigger to begin the import process for the uploaded set of CSVs.
This file must be uploaded last to indicate that the _Import Job_ should begin the import.

## CSV file specifications

There are 10 recognised CSV files:
[_uuidRemappings.csv_](#uuidremappingscsv), [_properties.csv_](#propertiescsv), [_groups.csv_](#groupscsv), [_units.csv_](#unitscsv), [_userRelations.csv_](#userrelationscsv)
[_utilisationPeriods.csv_](#utilisationperiodscsv), [_tenantCheckIns.csv_](#tenantcheckinscsv), [_tenants.csv_](#tenantscsv),
[_propertyTeams.csv_](#propertyteamscsv), [_agents.csv_](#agentscsv).
It is not required that each CSV be included in each Import Job.
For example, it is possible to include only the `agents.csv` file, or any other combination.
However, when inserting new data, the necessary data to resolve the foreign ID relationships _must_ also be included.
Issues in resolving data relationships will result in an error, terminating the complete Import Job.

#### Global Observations:

-   Columns can be in any order
-   First row in CSV file must contain column field name
-   Column field names are case-sensitive
-   Unless noted otherwise, all fields are required when _importType_ is `insert`
-   Only UUID type fields required when _importType_ is `update`
-   Files must use the UTF-8 character encoding

### uuidRemappings.csv

The `uuidRemappings.csv` file allows for the remapping of UUIDs on resources. **Note:** The `uuidRemappings.csv` cannot be combined with other CSV files within a single import. Doing so will result in an import error.

#### Fields

| Field          | Type                        | Description                               |
| -------------- | --------------------------- | ----------------------------------------- |
| **importType** | [Import Type](#import-type) | Always use "update"                       |
| **resource**   | [Resource](#resource-type)  | The type of resource these UUIDs refer to |
| **oldUuid**    | [UUID](#uuid-type)          | The previous UUID                         |
| **newUuid**    | [UUID](#uuid-type)          | The new UUID                              |

#### Example

```csv
importType,resource,oldUuid,newUuid
update,property,7cebf887-8914-4576-ade4-077f3869c17d,40456bad-f487-4a49-b94a-9d215ce32379
update,group,4FCC2A82-BB79-4CB2-B428-8EC6FF3200AF,bebf0794-9824-42cb-858b-f8365eddb560
update,unit,8a9428f6-3059-4358-b93b-f1b4d689f087,e3d2ffdf-0c07-489a-a290-2fb70336643b
update,utilisationPeriod,efd8b0f7-3e2e-4bc1-9bdb-7c5988835e68,76f3d8e4-70b5-49d0-9dc9-8b3a3a072629
```

### properties.csv

The `properties.csv` file imports property resource data.

#### Fields

| Field          | Type                        | Description                    |
| -------------- | --------------------------- | ------------------------------ |
| **importType** | [Import Type](#import-type) | One of:<br/>`insert`, `update` |
| **id**         | [UUID](#uuid-type)          | Your UUID for this property    |
| **name**       | [string](#string-type)      | The property name              |

#### Example

```csv
importType,id,name
insert,ab463b8b-a76c-4f6a-a726-75ab5730b69b,Christiannaurus Tower II
insert,9b86a4d7-ab95-4b65-a553-24fac1c60627,Nickalonius Gardens
insert,40456bad-f487-4a49-b94a-9d215ce32379,Achimalon Acres
```

### groups.csv

The `groups.csv` file imports group data.

#### Fields

| Field           | Type                            | Description                                                                    |
| --------------- | ------------------------------- | ------------------------------------------------------------------------------ |
| **importType**  | [Import Type](#import-type)     | One of:<br/>`insert`, `update`                                                 |
| **id**          | [UUID](#uuid-type)              | Your UUID for the group                                                        |
| **propertyId**  | [UUID](#uuid-type)              | The foreign UUID of the property the group belongs to (_properties.csv_ `id`)  |
| **name**        | [string](#string-type)          |
| **country**     | [Country](#country-type)        |
| **city**        | [string](#string-type)          |
| **streetName**  | [string](#string-type)          | The name of the street the structure is located on, e.g. Kaiser-Joseph-Strasse |
| **houseNumber** | [string](#string-type)          | The number of the structure on the street, e.g. 123                            |
| **zipCode**     | [Postal Code](#postalcode-type) |

#### Example

```csv
importType,city,country,houseNumber,id,name,propertyId,streetName,zipCode
insert,Kaylieshire,CA,838,80be246c-255d-480a-a132-f57a3d48dd86,Filomena Hills,ab463b8b-a76c-4f6a-a726-75ab5730b69b,Katrine Mount,12930-8559
insert,South Elda,IE,226,e94b98c5-7abd-4229-bd5f-c11603e7e831,Fisher,ab463b8b-a76c-4f6a-a726-75ab5730b69b,Konopelski Strasse,92671
insert,Edmundfurt,FR,551,2a7e8b67-370c-4994-8e99-c752e0c295c7,Senger,ab463b8b-a76c-4f6a-a726-75ab5730b69b,Brittany Ridge Rd.,107-0231
```

### units.csv

The `units.csv` file imports unit data. It allows for associating groups with units.

#### Fields

| Field          | Type                        | Description                                                           |
| -------------- | --------------------------- | --------------------------------------------------------------------- |
| **importType** | [Import Type](#import-type) | One of:<br/>`insert`, `update`                                        |
| **id**         | [UUID](#uuid-type)          | Your UUID for the unit                                                |
| **groupId**    | [UUID](#uuid-type)          | The foreign UUID of the group the unit belongs to (_groups.csv_ `id`) |
| **name**       | [string](#string-type)      |

#### Example

```csv
importType,groupId,id,name
insert,80be246c-255d-480a-a132-f57a3d48dd86,1b6e456c-1ba0-4f04-aed2-f228944836be,Lemon Drive 2
insert,80be246c-255d-480a-a132-f57a3d48dd86,d3288db2-99ed-47c3-b40c-ad3b82c76cb1,Kaiser-Joseph Strasse 8
insert,80be246c-255d-480a-a132-f57a3d48dd86,35762dd3-3e10-43f8-8ee1-9f8ff1fecfaf,Lollypop Lane 5
insert,80be246c-255d-480a-a132-f57a3d48dd86,06a13020-5b67-4579-9184-3629d707e75d,Blauen Quelle Strasse 7
```

### utilisationPeriods.csv

The `utilisationPeriods.csv` describes utilisation periods.

#### Fields

| Field          | Type                        | Description                                                                       |
| -------------- | --------------------------- | --------------------------------------------------------------------------------- |
| **importType** | [Import Type](#import-type) | One of:<br/>`insert`, `update`                                                    |
| **id**         | [UUID](#uuid-type)          | Your UUID for the utilisation period                                              |
| **unitId**     | [UUID](#uuid-type)          | The foreign UUID of the unit the utilisation period belongs to (_units.csv_ `id`) |
| **startDate**  | [Date](#date-type)          |
| **endDate**    | [Date](#date-type)          |

#### Example

```csv
importType,endDate,id,startDate,unitId
insert,2021-03-04,7d87a383-1778-401b-a421-b60c900479c3,1985-05-22,1b6e456c-1ba0-4f04-aed2-f228944836be
insert,2019-12-26,2ab719f8-8c1a-42d0-9078-001a8b77e259,1989-07-20,1b6e456c-1ba0-4f04-aed2-f228944836be
update,2018-12-07,5bd46ead-14f0-4b74-9a7a-12409e9dad59,,1b6e456c-1ba0-4f04-aed2-f228944836be
insert,2020-03-28,0171cf8e-ec7b-4a37-9786-b03645439e6b,1987-04-14,1b6e456c-1ba0-4f04-aed2-f228944836be
```

### tenants.csv

The `tenants.csv` file describes registration codes for a tenant.

#### Fields

| Field                | Type                               | Description                    |
| -------------------- | ---------------------------------- | ------------------------------ |
| **importType**       | [Import Type](#import-type)        | One of:<br/>`insert`, `update` |
| **id**               | [UUID](#uuid-type)                 | Your UUID for the tenant       |
| **registrationCode** | [string](#string-type)             |                                |
| **email**            | [string](#string-type)             | Optional                       |
| **phone**            | [Phone Number](#phone-number-type) | Optional                       |
| **name**             | [string](#string-type)             | Optional                       |
#### Example

```csv
importType,id,registrationCode,email,phone,name
insert,db8b732f-e0ff-41d9-9c15-ca1be2776fd4,3fdfdssf7,max@mustermann.de,+1700471246,Max Mustermann
insert,ad257d42-1078-4279-9918-e774859555ae,18ggjbs9d
insert,2f0fc4c6-3a5e-4cd7-9007-9add50653be5,ffhh3t4jg
insert,41f93044-e050-4a33-9e52-ebdada55f9a7,1awh74hgf
```

### tenantCheckIns.csv

The `tenantCheckIns.csv` file describes the relation between a tenant and a utilisation period.

#### Fields

| Field                   | Type                        | Description                                                                                           |
| ----------------------- | --------------------------- | ----------------------------------------------------------------------------------------------------- |
| **importType**          | [Import Type](#import-type) | One of:<br/>`insert`, `update`                                                                        |
| **utilisationPeriodId** | [UUID](#uuid-type)          | The foreign UUID of the utilisation period the tenant check-in is for (_utilisationPeriods.csv_ `id`) |
| **tenantId**            | [UUID](#uuid-type)          | The foreign UUID of the tenant tenant check-in is for (_tenantss.csv_ `id`)                           |

#### Example

```csv
importType,tenantId,utilisationPeriodId
insert,7d87a383-1778-401b-a421-b60c900479c3,db8b732f-e0ff-41d9-9c15-ca1be2776fd4
insert,7d87a383-1778-401b-a421-b60c900479c3,db8b732f-e0ff-41d9-9c15-ca1be2776fd4
insert,7d87a383-1778-401b-a421-b60c900479c3,ad257d42-1078-4279-9918-e774859555ae
insert,7d87a383-1778-401b-a421-b60c900479c3,ad257d42-1078-4279-9918-e774859555ae
```

### agents.csv

The `agents.csv` file describes agent-user account data.

#### Fields

| Field          | Type                               | Description                    |
| -------------- | ---------------------------------- | ------------------------------ |
| **importType** | [Import Type](#import-type)        | One of:<br/>`insert`, `update` |
| **id**         | [UUID](#uuid-type)                 | Your UUID for the agent        |
| **email**      | [string](#string-type)             |
| **firstName**  | [string](#string-type)             |
| **lastName**   | [string](#string-type)             |
| **phone**      | [Phone Number](#phone-number-type) | Optional                       |

#### Example

```csv
importType,email,firstName,id,lastName,phone
insert,orrin.welch@yahoo.test,Orrin,07955b8c-41ac-4a47-9157-3c6fb8450ef4,Welch,+1700471246
insert,ahaag@gmail.test,Ari,c3b41a1a-bc06-4a84-ba52-484b540b66e3,Haag,
update,sjudah@Amos.test,Saean,ea9012a3-3e98-4be3-8d60-6af255759962,Judah,+5643541048
update,tom@ming.test,Tom,da79623e-a03b-4c4f-a569-571ce4ac620b,Ming,
```

### propertyTeams.csv

The `propertyTeams.csv` file describes the relation between a property and an agent.

#### Fields

| Field             | Type                        | Description                                                                                           |
| ----------------- | --------------------------- | ----------------------------------------------------------------------------------------------------- |
| **importType**    | [Import Type](#import-type) | One of:<br/>`insert`, or `delete`                                                                     |
| **propertyId**    | [UUID](#uuid-type)          | The foreign UUID of the property that the team belongs to (_properties.csv_ `id`)                     |
| **agentId**       | [UUID](#uuid-type)          | The foreign UUID of the agent that belongs to the team (_agents.csv_ `id`)                            |
| **validFromDate** | [DateTime](#datetime-type)  | Optional start date of the validity of the relation (if provided **validToDate** is required as well) |
| **validToDate**   | [DateTime](#datetime-type)  | Optional end date of the validity of the relation (if provided **validFromDate** is required as well) |

#### Example

```csv
importType,agentId,propertyId,validFromDate,validToDate
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,ab463b8b-a76c-4f6a-a726-75ab5730b69b,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,ab463b8b-a76c-4f6a-a726-75ab5730b69b,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,9b86a4d7-ab95-4b65-a553-24fac1c60627,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,9b86a4d7-ab95-4b65-a553-24fac1c60627,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
```

### userRelations.csv

The `userRelations.csv` file describes the relation between an agent and a resource

#### Fields

| Field            | Type                            | Description                                                                                 |
| ---------------- | ------------------------------- | ------------------------------------------------------------------------------------------- |
| **importType**   | [Import Type](#import-type)     | One of:<br/>`insert`, or `delete`                                                           |
| **agentId**       | [UUID](#uuid-type)              | The foreign UUID of the agent that is responsible for the channel path (_agents.csv_ `id`)  |
| **resourceId**   | [UUID](#uuid-type)              | The foreign UUID of the resource that the agent belongs to ([resource](#resource-type).csv) |
| **resourceType** | [Resource Type](#resource-type) | The type of the resource being referenced by the resourceId (`property`)                    |
| **validFromDate** | [DateTime](#datetime-type)  | Optional start date of the validity of the relation (if provided **validToDate** is required as well) |
| **validToDate**   | [DateTime](#datetime-type)  | Optional end date of the validity of the relation (if provided **validFromDate** is required as well) |

#### Example

```csv
importType,agentId,resourceId,resourceType,validFromDate,validToDate
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,ab463b8b-a76c-4f6a-a726-75ab5730b69b,property,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,ab463b8b-a76c-4f6a-a726-75ab5730b69b,property,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,9b86a4d7-ab95-4b65-a553-24fac1c60627,property,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,9b86a4d7-ab95-4b65-a553-24fac1c60627,property,2019-01-01T00:00:00Z,2019-03-01T00:00:00Z
```

### manifest.json

The `manifest.json` file controls the Import Jobs execution and behavior.
The upload/presence of a `manifest.json` file at a Job Import upload location triggers the import process.
While Customer Support will set up default configuration for each ERP Import Customer, the `manifest.json` file also allows for some customisation of options.
These options are outlined here:

#### Fields

| Field                         | Type                                              | Description                                                                                                                                                       |
| ----------------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **agentPermissions**          | Array of Strings                                  | List of permissions to be set for agent imported in the Property Teams file E.g. to set permissions for pinboard and documents ['pinboardAdmin', 'documentAdmin'] |
| **autoImport**                | boolean                                           | Controls whether to automatically import, or to send confirmation email first                                                                                     |
| **locale**                    | ISO-639 Language Codes and ISO-3166 Country Codes | Default locale. E.g. locale for new agents. `en_US`, `de_DE`                                                                                                      |
| **receiveAdminNotifications** | boolean                                           | Receives Notification-Mails for Tickets with no Assignee. Default setting is true                                                                                 |
| **reportEmails**              | Array of Strings                                  | List of email addresses which should receive report emails for this job                                                                                           |
| **unitType**                  | string                                            | Controls the type of import Units, one of type 'rented' or 'owned'                                                                                                |
| **webhooks**                  | Array of [webhooks](#webhooks)                    | List of [webhooks](#webhooks) that can be called for each job, triggered by defined events.                                                                       |

#### Examples

```json
{
	"autoImport": false,
	"locale": "de_DE",
	"receiveAdminNotifications": false,
	"reportEmails": ["mr.foo@bar.test", "mrs.foo@bar.test"],
	"webhooks": [
		{
			"description": "allthings#slackbot-noise",
			"format": "slack",
			"triggers": true,
			"url": "https://hooks.slack.com/services/"
		}
	]
}
```

### webhooks

If webhooks are defined, each webhook is called when the defined event triggers. The webhooks can be defined in the import configuration or in the manifest.json.
The webhooks from the manifest.json are merged to the configuration.  
With this, a customer can define a webhook and also the event when it should be called.

The options webhooks needed are outlined here:

#### Fields

| Field           | Type                        | Description                                                                                                                                        |
| --------------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **description** | string                      | A description or name of the webhook.                                                                                                              |
| **format**      | string                      | Either `'slack'` or `'rest'`                                                                                                                       |
| **triggers**    | Array of strings OR boolean | Either an array of trigger-events or `true` to match all. Possible trigger events are: `'imported', 'created', 'failed', 'processing'`             |
| **url**         | string                      | The url of the webhook which is called with a `put` request. This url can include some parameters like f.e. `jobID` which are just passed through. |

#### Examples

```json
{
	"description": "custom-slack-channel",
	"format": "slack",
	"triggers": true,
	"url": "https://hooks.slack.com/services/cdsdfe3"
}
```

or

```json
{
	"description": "custom-rest-api",
	"format": "rest",
	"triggers": ["imported", "failed"],
	"url": "https://some/rest/endpoint"
}
```

### webhook-data

The payload that is sent to the rest api includes following fields:

#### Fields

| Field             | Type             | Description                                                                         |
| ----------------- | ---------------- | ----------------------------------------------------------------------------------- |
| **customerId**    | string           | The customerID, defined in the import configuration                                 |
| **jobCreateDate** | number           | The timestamp when the job was created                                              |
| **jobId**         | string           | The nisaba jobID                                                                    |
| **jobStatus**     | string           | The status of the job lik f.e.: `'importing', 'pending', 'processing', 'validated'` |
| **jobUpdateDate** | number           | The timestamp when the job was last updated                                         |
| **status**        | Array of strings | The list of the imported CSVs                                                       |
| **totalJobRows**  | number           | The total number of imported rows                                                   |

#### Example

```json
{
	"customerId": "foo-bar",
	"jobCreateDate": 1554465632000,
	"jobId": "sfd7we",
	"jobStatus": "finished",
	"jobUpdateDate": 1554465813000,
	"status": ["properties.csv"],
	"totalJobRows": 23
}
```

if no default option should be overwritten, the manifest.json should include at least an empty JSON-Object:

```json
{}
```

## Data Types

The following table describes the expected data formats of data provided in the CSV files.
Data is validated before import, and any errors are reported.
Import Jobs with validation errors are not processed.
In other words, a single invalid field will terminate the entire import process for all CSV files in the Import Job.

| Type                                                          | Description                                                                                     | Example                                                                                                                |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Country**<a name="country-type" />                          | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code.            | `CH`, `DE`, `FR`                                                                                                       |
| **Date**<a name="date-type" />                                | [ISO 8601 Calendar Date](https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) (`yyyy-mm-dd`)  | `2001-05-11`, `2018-03-06`, `2063-04-05`
| **DateTime**<a name="datetime-type" />                                | [ISO 8601 Combined date and time representation](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations)  | `2015-01-17T18:23:02+06:45`, `2015-01-17T18:23:02Z`                                                                                   |
| **Import Type**<a name="import-type" /><a name="date-type" /> | One of:<br/>`insert`, `update`, `delete`                                                        | `insert`                                                                                                               |
| **Phone Number**<a name="phone-number-type" />                | plus symbol `+` followed by only numbers (5-20 characters), no formatting                                         | `+4134567890`                                                                                                          |
| **Postal Code**<a name="postalcode-type" />                   | Only numbers and hyphens                                                                        | `123-4567`, `3457`, `93012`                                                                                            |
| **Resource**<a name="resource-type" />                        | One of:<br/>`property`, `group`, `unit`, `utilisationPeriod`                                    | `group`                                                                                                                |
| **String**<a name="string-type" />                            | any UTF-8 string                                                                                | `This is OK`, `This 1 is also good.`, `これも大丈夫`                                                                   |
| **UUID**<a name="uuid-type" />                                | [RFC 4122 version 4 (random) UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) | `71b63eef-3c50-4632-924c-b3c59f45c0ef`, `71fc860f-7fa9-4012-a32e-88be33d607af`, `bf9d6003-19a2-4c3e-b68c-828c45d2cf10` |

# ERP Data Exchange Specifications

This document specifies the Allthings ERP Data Exchange format.

## Contents

1.  [Import Process Outline](#import-process-outline)
1.  [CSV file specifications](#csv-file-specifications)
    1.  [uuid-remappings.csv](#uuid-remappings-csv)
    1.  [properties.csv](#properties-csv)
    1.  [groups.csv](#groups-csv)
    1.  [units.csv](#units-csv)
    1.  [utilisationPeriods.csv](#utilisationPeriods-csv)
    1.  [tenantCheckIns.csv](#tenantCheckIns-csv)
    1.  [tenants.csv](#tenants-csv)
    1.  [propertyTeams.csv](#propertyTeams-csv)
    1.  [agents.csv](#agents-csv)
    1.  [manifest.json](#properties-json)
1.  [Data Types](#data-types)
    1.  [Date Type](#date-type)
    1.  [Country Type](#country-type)
    1.  [Import Type Enumerable Type](#import-type-enumerable-type)
    1.  [Resource Enumerable Type](#resource-enumerable-type)
    1.  [String Type](#string-type)
    1.  [UUID Type](#uuid-type)

## Import Process Outline

bla bla bla todo something about csvs uploaded to an s3 bucket and put the trigger file last bla bla bla

## CSV file specifications

Using CSV files to import data bla bla bla bla todo here

#### Global Observations:

* Columns can be in any order
* First row must contain column field name
* Column field names are case-sensitive
* All fields are required when _importType_ is `insert`
* Only UUID type fields required when _importType_ is `update`

### uuid-remappings.csv

The `uuid-remappings.csv` file allows for the remapping of UUIDs on resources.

#### Fields

| Field        | Type                                  | Description                               |
| ------------ | ------------------------------------- | ----------------------------------------- |
| **resource** | [Resource](#resource-enumerable-type) | The type of resource these UUIDs refer to |
| **oldUuid**  | [UUID](#uuid-type)                    | The previous UUID                         |
| **newUuid**  | [UUID](#uuid-type)                    | The new UUID                              |

#### Example

```csv
resource,oldUuid,newUuid
property,F14C24EA-392B-4E2E-AC92-AE8F85BFCF85,35B642F4-3219-4FC6-8B7D-89A9EB433E1C
group,4FCC2A82-BB79-4CB2-B428-8EC6FF3200AF,9707F8D7-C020-4C7F-942D-CA4C3678D85F
unit,1F7E1E4B-B6B9-4CB0-B64F-F85827735F57,1C2AA5A4-5E7B-4479-97CE-6304C128F446
utilisationPeriod,ABC3B7A1-B0F5-4240-91DA-177FCF4E86D6,51238BDD-C9E0-4BEE-8142-EEA5D8A77D08
```

### properties.csv

The `properties.csv` file imports property resouce data.

#### Fields

| Field          | Type                                       | Description                       |
| -------------- | ------------------------------------------ | --------------------------------- |
| **importType** | [ImportType](#import-type-enumerable-type) |
| **id**         | [UUID](#uuid-type)                         | The foreign UUID of this property |
| **name**       | [string](#string-type)                     | The property name                 |

#### Example

```csv
importType,id,name
insert,ab463b8b-a76c-4f6a-a726-75ab5730b69b,Christiannaurus Tower II
insert,9b86a4d7-ab95-4b65-a553-24fac1c60627,Nickalonius Gardens
insert,40456bad-f487-4a49-b94a-9d215ce32379,Achimalon Acres
```

### groups.csv

Groups bla bla

#### Fields

| Field           | Type                                       | Description                                            |
| --------------- | ------------------------------------------ | ------------------------------------------------------ |
| **importType**  | [ImportType](#import-type-enumerable-type) |
| **id**          | [UUID](#uuid-type)                         | The foreign UUID of the group                          |
| **propertyId**  | [UUID](#uuid-type)                         | The foreign UUID of the property this group belongs to |
| **name**        | [string](#string-type)                     |
| **country**     | [Country](#country-type)                   |
| **city**        | [string](#string-type)                     |
| **streetName**  | [string](#string-type)                     |
| **houseNumber** | [string](#string-type)                     | @TODO rename to `address` ?                            |
| **zipCode**     | [Postal Code](#postal-code-type)           |

#### Example

```csv
importType,
```

### units.csv

Units bla bla

#### Fields

| Field          | Type                                       | Description                                        |
| -------------- | ------------------------------------------ | -------------------------------------------------- |
| **importType** | [ImportType](#import-type-enumerable-type) |
| **id**         | [UUID](#uuid-type)                         | The foreign UUID of the unit                       |
| **groupId**    | [UUID](#uuid-type)                         | The foreign UUID of the group this unit belongs to |
| **name**       | [string](#string-type)                     |
| **type**       | [Unit Type](#unit-type-enumerable-type)    | The type of the unit                               |

#### Example

```csv
importType,
```

### utilisationPeriods.csv

Utilisation Periods bla bla

#### Fields

| Field          | Type                                       | Description |
| -------------- | ------------------------------------------ | ----------- |
| **importType** | [ImportType](#import-type-enumerable-type) |
| **endDate**    | [Date](#date-type)                         |
| **id**         | [UUID](#uuid-type)                         |
| **startDate**  | [Date](#date-type)                         |
| **unitId**     | [UUID](#uuid-type)                         |

#### Example

```csv
importType,
```

### tenantCheckIns.csv

Tenant Check Ins bla bla

#### Fields

| Field                   | Type                                       | Description |
| ----------------------- | ------------------------------------------ | ----------- |
| **importType**          | [ImportType](#import-type-enumerable-type) |
| **tenantId**            | [UUID](#uuid-type)                         |
| **utilisationPeriodId** | [UUID](#uuid-type)                         |

#### Example

```csv
importType,
```

### tenants.csv

Tenants bla bla

#### Fields

| Field                | Type                                       | Description |
| -------------------- | ------------------------------------------ | ----------- |
| **importType**       | [ImportType](#import-type-enumerable-type) |
| **id**               | [UUID](#uuid-type)                         |
| **registrationCode** | [string](#string-type)                     |

#### Example

```csv
importType,
```

### propertyTeams.csv

Property Teams bla bla

#### Fields

| Field          | Type                                       | Description |
| -------------- | ------------------------------------------ | ----------- |
| **importType** | [ImportType](#import-type-enumerable-type) |
| **agentId**    | [UUID](#uuid-type)                         |
| **agentType**  | [string](#string-type)                     |
| **propertyId** | [UUID](#uuid-type)                         |

#### Example

```csv
importType,
```

### agents.csv

Agents bla bla

#### Fields

| Field          | Type                                       | Description                   |
| -------------- | ------------------------------------------ | ----------------------------- |
| **importType** | [ImportType](#import-type-enumerable-type) |
| **company**    | [string](#string-type)                     |
| **email**      | [string](#string-type)                     |
| **firstName**  | [string](#string-type)                     |
| **gender**     | [Gender](#gender-enumerable-type)          | @TODO do we really need this? |
| **id**         | [UUID](#uuid-type)                         |
| **lastName**   | [string](#string-type)                     |
| **phone**      | [string](#string-type)                     |

#### Example

```csv
importType,
```

### manifest.json

The manifest.json file bla bla triggers and stuff and also controls some config per-import. Fields omitted fields inherit from customer-specific global configuration.

#### Fields

| Field           | Type         | Description                                                                   |
| --------------- | ------------ | ----------------------------------------------------------------------------- |
| **autoImport**  | boolean      | Controls whether to automatically import, or to send confirmation email first |
| **reportEmail** | Array<Email> | List of email addresses which should receive report emails for this job       |

#### Example

```json
{
  "autoImport": false,
  "reportEmail": ["mr.foo@bar.test", "mrs.foo@bar.test"]
}
```

## Data Types

### Date Type

[ISO 8601 Calendar Date](https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) (`yyyy-mm-dd`)

E.g. `2001-05-11`, `2018-03-06`, `2063-04-05`

### Country Type

[ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code.

E.g. `CH`, `DE`, `FR`

### Gender Enumerable Type

@TODO do we really need this field?

One of: `male`, `female`, `non-binary`

### Import Type Enumerable Type

One of: `insert`, `update`

### Postal Code Type

@TODO

E.g. `123-4567`, `3457`, `93012`

### Resource Enumerable Type

One of: `property`, `group`, `unit`, `utilisationPeriod`

### String Type

Any UTF-8 string

E.g. `This is OK`, `This 1 is also good.`, `これも大丈夫`

### Unit Type Enumerable Type

@TODO

One of: `FIXME`, `FIXME`, `@TODO`

### UUID Type

[RFC 4122 version 4 (random) UUID](<https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_4_(random)>)

E.g. `71b63eef-3c50-4632-924c-b3c59f45c0ef`, `71fc860f-7fa9-4012-a32e-88be33d607af`, `bf9d6003-19a2-4c3e-b68c-828c45d2cf10`

# ERP Data Exchange Specifications

This document specifies the Allthings ERP Data Exchange format.

## Contents

1.  [Import Process Outline](#import-process-outline)
1.  [CSV file specifications](#csv-file-specifications)
    1.  [uuid-remappings.csv](#uuid-remappingscsv)
    1.  [properties.csv](#propertiescsv)
    1.  [groups.csv](#groupscsv)
    1.  [units.csv](#unitscsv)
    1.  [utilisationPeriods.csv](#utilisationPeriodscsv)
    1.  [tenantCheckIns.csv](#tenantCheckInscsv)
    1.  [tenants.csv](#tenantscsv)
    1.  [propertyTeams.csv](#propertyTeamscsv)
    1.  [agents.csv](#agentscsv)
    1.  [manifest.json](#propertiesjson)
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

Using CSV files to import data bla bla bla bla todo here

#### Global Observations:

* Columns can be in any order
* First row in CSV file must contain column field name
* Column field names are case-sensitive
* All fields are required when _importType_ is `insert`
* Only UUID type fields required when _importType_ is `update`

### uuid-remappings.csv

The `uuid-remappings.csv` file allows for the remapping of UUIDs on resources.

#### Fields

| Field        | Type                       | Description                               |
| ------------ | -------------------------- | ----------------------------------------- |
| **resource** | [Resource](#resource-type) | The type of resource these UUIDs refer to |
| **oldUuid**  | [UUID](#uuid-type)         | The previous UUID                         |
| **newUuid**  | [UUID](#uuid-type)         | The new UUID                              |

#### Example

```csv
resource,oldUuid,newUuid
property,F14C24EA-392B-4E2E-AC92-AE8F85BFCF85,35B642F4-3219-4FC6-8B7D-89A9EB433E1C
group,4FCC2A82-BB79-4CB2-B428-8EC6FF3200AF,9707F8D7-C020-4C7F-942D-CA4C3678D85F
unit,1F7E1E4B-B6B9-4CB0-B64F-F85827735F57,1C2AA5A4-5E7B-4479-97CE-6304C128F446
utilisationPeriod,ABC3B7A1-B0F5-4240-91DA-177FCF4E86D6,51238BDD-C9E0-4BEE-8142-EEA5D8A77D08
```

### properties.csv

The `properties.csv` file imports property resource data.

#### Fields

| Field          | Type                        | Description                       |
| -------------- | --------------------------- | --------------------------------- |
| **importType** | [Import Type](#import-type) |
| **id**         | [UUID](#uuid-type)          | The foreign UUID of this property |
| **name**       | [string](#string-type)      | The property name                 |

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

| Field           | Type                            | Description                                            |
| --------------- | ------------------------------- | ------------------------------------------------------ |
| **importType**  | [Import Type](#import-type)     |
| **id**          | [UUID](#uuid-type)              | The foreign UUID of the group                          |
| **propertyId**  | [UUID](#uuid-type)              | The foreign UUID of the property this group belongs to |
| **name**        | [string](#string-type)          |
| **country**     | [Country](#country-type)        |
| **city**        | [string](#string-type)          |
| **streetName**  | [string](#string-type)          |
| **houseNumber** | [string](#string-type)          | @TODO rename to `streetNumber` ?                       |
| **zipCode**     | [Postal Code](#postalcode-type) |

#### Example

```csv
importType,city,country,houseNumber,id,name,propertyId,streetName,zipCode
insert,Kaylieshire,CA,Apt. 838,80be246c-255d-480a-a132-f57a3d48dd86,Filomena Hills,ab463b8b-a76c-4f6a-a726-75ab5730b69b,999 Katrine Mount,12930-8559
insert,South Elda,IE,Suite 226,e94b98c5-7abd-4229-bd5f-c11603e7e831,Fisher,ab463b8b-a76c-4f6a-a726-75ab5730b69b,78 Konopelski Run Suite 979,92671
insert,Edmundfurt,FR,Suite 551,2a7e8b67-370c-4994-8e99-c752e0c295c7,Senger,ab463b8b-a76c-4f6a-a726-75ab5730b69b,4902 Brittany Ridge Apt. 249,32416-2182
```

### units.csv

The `units.csv` file imports unit data. It allows for associating groups with units.

#### Fields

| Field          | Type                        | Description                                        |
| -------------- | --------------------------- | -------------------------------------------------- |
| **importType** | [Import Type](#import-type) |
| **id**         | [UUID](#uuid-type)          | The foreign UUID of the unit                       |
| **groupId**    | [UUID](#uuid-type)          | The foreign UUID of the group this unit belongs to |
| **name**       | [string](#string-type)      |

#### Example

```csv
importType,groupId,id,name
insert,80be246c-255d-480a-a132-f57a3d48dd86,1b6e456c-1ba0-4f04-aed2-f228944836be,Mrs. Giles Sporer @TODO property name
insert,80be246c-255d-480a-a132-f57a3d48dd86,d3288db2-99ed-47c3-b40c-ad3b82c76cb1,Ms. Shanie Leuschke
insert,80be246c-255d-480a-a132-f57a3d48dd86,35762dd3-3e10-43f8-8ee1-9f8ff1fecfaf,Ms. Lauren Koelpin
insert,80be246c-255d-480a-a132-f57a3d48dd86,06a13020-5b67-4579-9184-3629d707e75d,Mrs. Sophie Hagenes
```

### utilisationPeriods.csv

The `utilisationPeriods.csv` describes utilisation periods.

#### Fields

| Field          | Type                        | Description |
| -------------- | --------------------------- | ----------- |
| **importType** | [Import Type](#import-type) |
| **id**         | [UUID](#uuid-type)          |
| **unitId**     | [UUID](#uuid-type)          |
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

| Field                | Type                        | Description |
| -------------------- | --------------------------- | ----------- |
| **importType**       | [Import Type](#import-type) |
| **id**               | [UUID](#uuid-type)          |
| **registrationCode** | [string](#string-type)      |

#### Example

```csv
importType,id,registrationCode
insert,db8b732f-e0ff-41d9-9c15-ca1be2776fd4,37
insert,ad257d42-1078-4279-9918-e774859555ae,189
insert,2f0fc4c6-3a5e-4cd7-9007-9add50653be5,34
insert,41f93044-e050-4a33-9e52-ebdada55f9a7,174
```

### tenantCheckIns.csv

The `tenantCheckIns.csv` file describes the relation between a tenant and a utilisation period.

#### Fields

| Field                   | Type                        | Description |
| ----------------------- | --------------------------- | ----------- |
| **importType**          | [Import Type](#import-type) |
| **utilisationPeriodId** | [UUID](#uuid-type)          |
| **tenantId**            | [UUID](#uuid-type)          |

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

| Field          | Type                               | Description |
| -------------- | ---------------------------------- | ----------- |
| **importType** | [Import Type](#import-type)        |
| **id**         | [UUID](#uuid-type)                 |
| **email**      | [string](#string-type)             |
| **firstName**  | [string](#string-type)             |
| **lastName**   | [string](#string-type)             |
| **phone**      | [Phone Number](#phone-number-type) |
| **company**    | [string](#string-type)             |

#### Example

```csv
importType,company,email,firstName,id,lastName,phone
insert,Mueller Group,Amani_Williamson@yahoo.com,Orrin,07955b8c-41ac-4a47-9157-3c6fb8450ef4,Welch,+1700471246
insert,Douglas Ltd,Priscilla_Koelpin@yahoo.com,Ari,c3b41a1a-bc06-4a84-ba52-484b540b66e3,Haag,+9679915262
update,,Schoen_Judah@Amos.me,,ea9012a3-3e98-4be3-8d60-6af255759962,,+5643541048
update,,,,da79623e-a03b-4c4f-a569-571ce4ac620b,,+8804272042
```

### propertyTeams.csv

The `propertyTeams.csv` file describes the relation between a property and an agent.

#### Fields

| Field          | Type                        | Description |
| -------------- | --------------------------- | ----------- |
| **importType** | [Import Type](#import-type) |
| **propertyId** | [UUID](#uuid-type)          |
| **agentId**    | [UUID](#uuid-type)          |

#### Example

```csv
importType,agentId,propertyId
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,ab463b8b-a76c-4f6a-a726-75ab5730b69b
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,ab463b8b-a76c-4f6a-a726-75ab5730b69b
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,9b86a4d7-ab95-4b65-a553-24fac1c60627
insert,07955b8c-41ac-4a47-9157-3c6fb8450ef4,9b86a4d7-ab95-4b65-a553-24fac1c60627
```

### manifest.json

The `manifest.json` file controls the Import Jobs execution and behavior.
The upload/presence of a `manifest.json` file at a Job Import upload location triggers the import process.
While Customer Support will set up default configuration for each ERP Import Customer, the `manifest.json` file also allows for some customisation of options.
These options are outlined here:

#### Fields

| Field            | Type         | Description                                                                   |
| ---------------- | ------------ | ----------------------------------------------------------------------------- |
| **autoImport**   | boolean      | Controls whether to automatically import, or to send confirmation email first |
| **reportEmails** | Array<Email> | List of email addresses which should receive report emails for this job       |

#### Example

```json
{
  "autoImport": false,
  "reportEmails": ["mr.foo@bar.test", "mrs.foo@bar.test"]
}
```

## Data Types

The following table describes the expected data formats of data provided in the CSV files.
Data is validated before import, and any errors are reported.
Import Jobs with validation errors are not processed.
In other words, a single invalid field will terminate the entire import process for all CSV files in the Import Job.

| Type                                                          | Description                                                                                                          | Example                                                                                                                |
| ------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Country**<a name="country-type" />                          | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code.                                 | `CH`, `DE`, `FR`                                                                                                       |
| **Date**<a name="date-type" />                                | [ISO 8601 Calendar Date](https://en.wikipedia.org/wiki/ISO_8601#Calendar_dates) (`yyyy-mm-dd`)                       | `2001-05-11`, `2018-03-06`, `2063-04-05`                                                                               |
| **Import Type**<a name="import-type" /><a name="date-type" /> | One of:<br/>`insert`, `update`                                                                                       | `insert`                                                                                                               |
| **Phone Number**<a name="phone-number-type" />                | plus symbol `+` followed by only numbers, no formatting                                                              | `+4134567890`                                                                                                          |
| **Postal Code**<a name="postalcode-type" />                   | Only numbers and hyphens                                                                                             | `123-4567`, `3457`, `93012`                                                                                            |
| **Resource**<a name="resource-type" />                        | One of:<br/>`property`, `group`, `unit`, `utilisationPeriod`                                                         | `group`                                                                                                                |
| **String**<a name="string-type" />                            | any UTF-8 string                                                                                                     | `This is OK`, `This 1 is also good.`, `これも大丈夫`                                                                   |
| **UUID**<a name="uuid-type" />                                | [RFC 4122 version 4 (random) UUID](<https://en.wikipedia.org/wiki/Universally*unique_identifier#Version_4*(random)>) | `71b63eef-3c50-4632-924c-b3c59f45c0ef`, `71fc860f-7fa9-4012-a32e-88be33d607af`, `bf9d6003-19a2-4c3e-b68c-828c45d2cf10` |

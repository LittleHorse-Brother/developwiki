# Contact

## Contact

* `GET /api/v3/globalSettings/contacts` - [Get a list of contacts in site](broken-reference)
* `GET /api/v3/globalSettings/contacts/{id}` - [Get a contact by id](broken-reference)
* `POST /api/v3/globalSettings/contacts` - [Create a new contact](broken-reference)
* `PUT /api/v3/globalSettings/contacts/{id}` - [Update a contact](broken-reference)
* `DELETE /api/v3/globalSettings/contacts/{id}` - [Delete a contact](broken-reference)

### Contact Related Object JSON format

#### Contact Object

| Name                | Type                                   | Include | Read-only | Mandatory | Default | Description                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------- | -------------------------------------- | :-----: | :-------: | :-------: | :-----: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                | integer                                |         |    yes    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `name`              | string                                 |         |     no    |    yes    |         | Contact Name can be edited by Agents. Default value is read from the first Identity. Only when a Contact sends a message in a specific channel that has a Name and an Avatar, like a Facebook Account, will the display Name and Avatar be used instead of the drawn from the contact profile. In all other situations, display will be drawn from the Contact Name and Avatar. |
| `description`       | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `firstName`         | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `lastName`          | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `alias`             | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `title`             | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `company`           | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `fax`               | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `phone`             | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `mailingAddress`    | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `city`              | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `stateOrProvince`   | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `countryOrRegion`   | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `postalOrZipCode`   | string                                 |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `timeZone`          | string                                 |         |     no    |     no    |         | Time zone of contact. Value includes all [Time Zone Option](broken-reference) identifiers.                                                                                                                                                                                                                                                                                      |
| `createdTime`       | DateTime                               |         |     no    |     no    |         | When the contact is created.                                                                                                                                                                                                                                                                                                                                                    |
| `lastUpdatedTime`   | DateTime                               |         |     no    |     no    |         |                                                                                                                                                                                                                                                                                                                                                                                 |
| `contactIdentities` | [ContactIdentity](broken-reference)\[] |   yes   |     no    |     no    |         | Contact Identities.                                                                                                                                                                                                                                                                                                                                                             |

#### Contact List Response Object

Agent List Object for agent list Response, include count and page information.

| Name           | Type                           | Include | Read-only | Mandatory | Default | Description                         |
| -------------- | ------------------------------ | :-----: | :-------: | :-------: | :-----: | ----------------------------------- |
| `count`        | integer                        |    no   |    yes    |     no    |         | Total count of the query.           |
| `nextPage`     | string                         |    no   |    yes    |     no    |         | The next page url of the query.     |
| `previousPage` | string                         |    no   |    yes    |     no    |         | The previous page url of the query. |
| `contacts`     | [Contact](broken-reference)\[] |    no   |    yes    |     no    |         | A list of contacts.                 |

### Contact Endpoints

#### Get all contacts

`GET /api/v3/globalSettings/contacts`

**Parameters**

Query string

| Name                   | Type    | Required | Default | Description                                                  |
| ---------------------- | ------- | -------- | ------- | ------------------------------------------------------------ |
| `name`                 | string  | no       |         | Contact name.                                                |
| `company`              | string  | no       |         | Contact company.                                             |
| `contactIdentityName`  | string  | no       |         | Name of the contact identity.                                |
| `contactIdentityValue` | string  | no       |         | Value of the contact identity.                               |
| `contactIdentityType`  | string  | no       |         | Type of the contact identity.                                |
| `keywords`             | string  | no       |         | Search scope includes: name, company, identity value, alias. |
| `include`              | string  | no       |         | Available value: `contactIdentity`                           |
| `pageIndex`            | integer | no       | 1       | The page index of the query.                                 |
| `pageSize`             | integer | no       | 10      | The page size of the query.                                  |

**Response**

The response is a [Contact List Response](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contacts?name=Vincent
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "count": 1234,
  "nextPage": "https://api1.comm100.io/api/v3/globalSettings/contacts?name=Vincent&pageIndex=2",
  "previousPage": "",
  "contacts": [{
    "id": 7,
    "name": "Vincent",
    "description": "Accept Chats",
    "firstName": "Vincent",
    "lastName": "Crabbe",
    "alias": "",
    "title": "CEO",
    "company": "BMW",
    ...
  },
  ...
  ]
}
```

#### Get a contact

`GET /api/v3/globalSettings/contacts/{id}`

**Parameters**

Path parameters

| Name | Type    | Required | Description                |
| ---- | ------- | -------- | -------------------------- |
| `id` | integer | yes      | Identifier of the contact. |

Query string

| Name      | Type   | Required | Default | Description                         |
| --------- | ------ | -------- | ------- | ----------------------------------- |
| `include` | string | no       |         | Available value: `contactIdentity`. |

**Response**

The response is a [Contact](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contacts/7
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": 7,
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}
```

#### Create a new contact

`POST /api/v3/globalSettings/contacts`

**Parameters**

Request body

The request body contains data with the [Contact](broken-reference) structure.

example:

```json
{
      "name": "Vincent",
      "description": "Accept Chats",
      "firstName": "Vincent",
      "lastName": "Crabbe",
      "alias": "",
      "title": "CEO",
      "company": "BMW",
      ...
    }
```

**Response**

The response is a [Contact](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -d '{
      "name": "Vincent",
      "description": "Accept Chats",
      "firstName": "Vincent",
      "lastName": "Crabbe",
      "alias": "",
      "title": "CEO",
      "company": "BMW",
      ...
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/contacts
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contacts/7
{
  "id": 7,
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}
```

#### Update a contact

`PUT /api/v3/globalSettings/contacts/{id}`

**Parameters**

Path parameters

| Name | Type    | Required | Description                |
| ---- | ------- | -------- | -------------------------- |
| `id` | integer | yes      | Identifier of the contact. |

Request body

The request body contains data with the [Contact](broken-reference) structure.

example:

```json
{
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}
```

**Response**

The response is a [Contact](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -d '{
  "name": "Vincent",
  "description": "Accept Chats",
  "firstName": "Vincent",
  "lastName": "Crabbe",
  "alias": "",
  "title": "CEO",
  "company": "BMW",
  ...
}' -X PUT https://api1.comm100.io/api/v3/globalSettings/contacts
```

Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contacts/7
{
    "id": 7,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    ...
}
```

#### Delete a contact

`DELETE /api/v3/globalSettings/contacts/{id}`

**Parameters**

Path parameters

| Name | Type    | Required | Description                |
| ---- | ------- | -------- | -------------------------- |
| `id` | integer | yes      | Identifier of the contact. |

**Response**

HTTP/1.1 204 No Content

**Example**

Using curl

```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/contacts/7
```

Response

```json
HTTP/1.1 204 No Content
```

## Contact Identity

* `GET /api/v3/globalSettings/contacts/{contactId}/contactIdentities` - [Get a list of contact identities in a contact](broken-reference)
* `GET /api/v3/globalSettings/contactIdentities/{id}` - [Get an contact identity by id](broken-reference)
* `POST /api/v3/globalSettings/contactIdentities` - [create a new contact identity](broken-reference)
* `PUT /api/v3/globalSettings/contactIdentities/{id}` - [update an contact identity](broken-reference)
* `DELETE /api/v3/globalSettings/contactIdentities/{id}` - [delete an contact identity](broken-reference)

### Contact Identity Related Object JSON format

#### Contact Identity Object

| Name                     | Type    | Include | Read-only | Mandatory | Default | Description                                                                                                                                                                                                                     |
| ------------------------ | ------- | :-----: | :-------: | :-------: | :-----: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                     | integer |         |    yes    |     no    |         |                                                                                                                                                                                                                                 |
| `contactId`              | integer |         |     no    |    yes    |         | Mandatory when post by contact identity api.                                                                                                                                                                                    |
| `name`                   | string  |         |     no    |     no    |         | The name used for a specific channel, like the name of a Facebook user. Not every channel will yield a name. For example, SMS numbers will display a number instead of a name.                                                  |
| `type`                   | string  |         |     no    |    yes    |         | The options for the value are: visitor, email, SMS, Facebook, Twitter, WeChat, ssoid, externalid, Whatsapp, inphase.                                                                                                            |
| `value`                  | string  |         |     no    |    yes    |         | The value of the identity.                                                                                                                                                                                                      |
| `avatarURL`              | string  |         |     no    |     no    |         | The avatar used in a certain channel, like the avatar of a Facebook user. Not every channel yields an avatar. For example, SMS Numbers won't produce one.                                                                       |
| `infoURL`                | string  |         |     no    |     no    |         | Contact information from the channels. Such as the number of Twitter followers and tweets from the twitter identity. The info is displayed in an iframe in the agent console. Available for Twitter, Facebook, SMS, and WeChat. |
| `screenName`             | string  |         |     no    |     no    |         | Twitter only. Like @Comm100Corp.                                                                                                                                                                                                |
| `originalContactPageURL` | string  |         |     no    |     no    |         | The contact profile URL on Facebook or Twitter.                                                                                                                                                                                 |

### Contact Identity Endpoints

#### Get all contact identity

`GET /api/v3/globalSettings/contacts/{contactId}/contactIdentities`

**Parameters**

Path parameters

| Name        | Type    | Required | Description                                                    |
| ----------- | ------- | -------- | -------------------------------------------------------------- |
| `contactId` | integer | yes      | Identifier of the contact, which Contact Identities belong to. |

**Response**

The response is a list of [Contact Identity](broken-reference) Objects.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contacts/7/contactIdentities
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json[
{
  "id": 25,
  "contactId": 7,
  "name": "Vincent",
  "type": "Visitor",
  "value": "",
  "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
  "infoURL": "",
  "screenName": "@Comm100Corp",
  "originalContactPageURL": ""
},
...,
]
```

#### Get a contact identity

`GET /api/v3/globalSettings/contactIdentities/{id}`

**Parameters**

Path parameters

| Name | Type    | Required | Description                        |
| ---- | ------- | -------- | ---------------------------------- |
| `id` | integer | yes      | Identifier of the contact identity |

**Response**

The response is a [Contact Identity](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": 25,
  "contactId": 7,
  "name": "Vincent",
  "type": "Visitor",
  "value": "",
  "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
  "infoURL": "",
  "screenName": "@Comm100Corp",
  "originalContactPageURL": ""
}
```

#### Create a new contact identity

`POST /api/v3/globalSettings/contacts/contactIdentities`

**Parameters**

Request body

The request body contains data with the [Contact Identity](broken-reference) structure.

example:

```json
 {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

**Response**

The response is a [Contact Identity](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -d ' {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/contacts/contactIdentities
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
{
    "id": 25,
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

#### Update a contact identity

`PUT /api/v3/globalSettings/contactIdentities/{id}`

**Parameters**

Path parameters

| Name | Type    | Required | Description                         |
| ---- | ------- | -------- | ----------------------------------- |
| `id` | integer | yes      | Identifier of the contact identity. |

Request body

The request body contains data with the [Contact Identity](broken-reference) structure.

example:

```json
 {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

**Response**

The response is a [Contact Identity](broken-reference) Object.

**Example**

Using curl

```
curl -H "Content-Type: application/json" -d ' {
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
```

Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
 {
    "id": 25,
    "contactId": 7,
    "name": "Vincent",
    "type": "Visitor",
    "value": "",
    "avatarURL": "https://bot.comm100.com/api/v3/chatbot/images/42dwdaww-92e6-4487-a2e8-92e68d6892e6",
    "infoURL": "",
    "screenName": "@Comm100Corp",
    "originalContactPageURL": ""
  }
```

#### Delete a contact identity

`DELETE /api/v3/globalSettings/contactIdentities/{id}`

**Parameters**

Path parameters

| Name | Type    | Required | Description                         |
| ---- | ------- | -------- | ----------------------------------- |
| `id` | integer | yes      | Identifier of the contact identity. |

**Response**

HTTP/1.1 204 No Content

**Example**

Using curl

```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/contactIdentities/25
```

Response

```json
HTTP/1.1 204 No Content
```

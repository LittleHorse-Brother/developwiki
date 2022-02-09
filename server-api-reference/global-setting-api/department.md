# Department

You need the `Manage departments` permission to manage departments.

* `GET /api/v3/globalSettings/departments` - [Get a list of departments in site](broken-reference)
* `GET /api/v3/globalSettings/departments/{id}` - [Get a department by id](broken-reference)
* `POST /api/v3/globalSettings/departments` - [Create a new department](broken-reference)
* `PUT /api/v3/globalSettings/departments/{id}` - [Update a department](broken-reference)
* `DELETE /api/v3/globalSettings/departments/{id}` - [Delete a department](broken-reference)

## Department Related Object JSON format

### Department Object

| Name                                 | Type                         | Include | Read-only | Mandatory |            Default           | Description                                                                                                                                                      |
| ------------------------------------ | ---------------------------- | :-----: | :-------: | :-------: | :--------------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                 | Guid                         |         |    yes    |     no    |                              |                                                                                                                                                                  |
| `name`                               | string                       |         |     no    |    yes    |                              |                                                                                                                                                                  |
| `description`                        | string                       |         |     no    |     no    |                              |                                                                                                                                                                  |
| `isAvailableInChat`                  | bool                         |         |     no    |     no    |             false            | When it is false, the Department will not be displayed in the Pre-chat window, Department drop down list, routing rules, chat transfer rules etc. Default: true. |
| `isAvailableInTicketingAndMessaging` | bool                         |         |     no    |     no    |             false            | When it is false, the department name will not be displayed in the 'Assigned Department' field. Default: true.                                                   |
| `offlineMessageMailTo`               | string                       |         |     no    |     no    | All agents in the department | The value options: All agents in the department, The email address(es).                                                                                          |
| `offlineMessageEmailAddresses`       | string                       |         |     no    |     no    |                              | Specific email addresses that offline message are send to. Available and required when Offline Message Mail Type is ‘The email address(es)’.                     |
| `agentIds`                           | int\[]                       |         |     no    |     no    |             NULL             | The selected agents for this department.                                                                                                                         |
| `agents`                             | [Agent](broken-reference)\[] |   yes   |     no    |     no    |                              |                                                                                                                                                                  |
| `shiftIds`                           | Guid\[]                      |         |     no    |     no    |             NULL             | The list of the shift ids which the department belongs to.                                                                                                       |
| `shifts`                             | [Shift](broken-reference)\[] |   yes   |     no    |     no    |                              |                                                                                                                                                                  |

## Department Endpoints

### get all departments

`GET /api/v3/globalSettings/departments`

#### Parameters

Query string

| Name      | Type   | Required | Default                          | Description |
| --------- | ------ | -------- | -------------------------------- | ----------- |
| `include` | string | no       | Available value:`agent`,`shift`. |             |

#### Response

The response is a list of [Department](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/departments
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
[{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
},
...,
]
```

### get a department

`GET /api/v3/globalSettings/departments/{id}`

#### Parameters

Path parameters

| Name | Type | Required | Description                   |
| ---- | ---- | -------- | ----------------------------- |
| `id` | Guid | yes      | Identifier of the department. |

Query string

| Name      | Type   | Required | Default                          | Description |
| --------- | ------ | -------- | -------------------------------- | ----------- |
| `include` | string | no       | Available value:`agent`,`shift`. |             |

#### Response

The response is a [Department](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/departments/bs22qa68-92e6-4487-a2e8-8234fc9d1f48
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```

### Create a new department

`POST /api/v3/globalSettings/departments`

#### Parameters

Request body

The request body contains data with the [Department](broken-reference) structure.

example:

```json
 {
      "name": "markting",
      "description": "markting departments",
      "isAvailableInChat": "yes",
      "isAvailableInTicketingAndMessaging": "yes",
      "offlineMessageMailTo": "All agents in the department",
      "offlineMessageEmailAddresses": "",
      "agentIds":  [
        68,
        ...,
      ],
      ...
    }
```

#### Response

The response is a [Department](broken-reference) object

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
      "name": "markting",
      "description": "markting departments",
      "isAvailableInChat": "yes",
      "isAvailableInTicketingAndMessaging": "yes",
      "offlineMessageMailTo": "All agents in the department",
      "offlineMessageEmailAddresses": "",
      "agentIds":  [
        68,
        ...,
      ],
      ...
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/roles
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/departments
{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```

### update a department

`PUT /api/v3/globalSettings/departments/{id}`

#### Parameters

Path parameters

| Name | Type | Required | Description                   |
| ---- | ---- | -------- | ----------------------------- |
| `id` | Guid | yes      | Identifier of the department. |

Request body

The request body contains data with the [Department](broken-reference) structure.

example:

```json
{
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```

#### Response

The response is a [Department](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}' -X PUT https://api1.comm100.io/api/v3/globalSettings/departments/bs22qa68-92e6-4487-a2e8-8234fc9d1f48
```

Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/departments/bs22qa68-92e6-4487-a2e8-8234fc9d1f48
{
  "id": "bs22qa68-92e6-4487-a2e8-8234fc9d1f48",
  "name": "markting",
  "description": "markting departments",
  "isAvailableInChat": "yes",
  "isAvailableInTicketingAndMessaging": "yes",
  "offlineMessageMailTo": "All agents in the department",
  "offlineMessageEmailAddresses": "",
  "agentIds":  [
    68,
    ...,
  ],
  ...
}
```

### Delete a department

`DELETE /api/v3/globalSettings/departments/{id}`

#### Parameters

Path parameters

| Name | Type | Required | Description                   |
| ---- | ---- | -------- | ----------------------------- |
| `id` | Guid | yes      | Identifier of the department. |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/departments/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```

Response

```json
HTTP/1.1 204 No Content
```

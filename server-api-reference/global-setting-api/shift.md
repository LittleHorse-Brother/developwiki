# Shift

You need `Manage Security` permission to set shift for a site.

* `GET /api/v3/globalSettings/shifts` - [Get all shifts in site](broken-reference)
* `GET /api/v3/globalSettings/departments/{departmentId}/shifts` - [Get all shifts by department id](broken-reference)
* `GET /api/v3/globalSettings/agents/{agentId}/shifts` - [Get all shifts by agent id](broken-reference)
* `GET /api/v3/globalSettings/shifts/{id}` - [Get a shift by id](broken-reference)
* `POST /api/v3/globalSettings/shifts` - [create a new shift](broken-reference)
* `PUT /api/v3/globalSettings/shifts/{id}` - [update a shift](broken-reference)
* `DELETE /api/v3/globalSettings/shifts/{id}` - [delete a shift](broken-reference)

## Related Object JSON format

### Shift Object

Shift Object is represented as simple flat JSON objects with the following keys:

| Name                   | Type                                 | Include | Read-only | Mandatory | Default | Description                                                                             |
| ---------------------- | ------------------------------------ | ------- | :-------: | :-------: | :-----: | --------------------------------------------------------------------------------------- |
| `id`                   | Guid                                 |         |    yes    |     no    |         | Identifier of the current item.                                                         |
| `name`                 | string                               |         |     no    |    yes    |         | Name of the shift.                                                                      |
| `timeZone`             | string                               |         |     no    |     no    |         | Time zone of shift. Value include all [Time Zone Option](broken-reference) identifiers. |
| `ifAutoDetectDayLight` | boolean                              |         |     no    |     no    |         |                                                                                         |
| `holidays`             | [Holiday](broken-reference)\[]       |         |     no    |     no    |         |                                                                                         |
| `agentIds`             | int\[]                               |         |    yes    |     no    |   NULL  |                                                                                         |
| `departmentIds`        | Guid\[]                              |         |    yes    |     no    |   NULL  |                                                                                         |
| `agents`               | [Agent](broken-reference)\[]         | yes     |     no    |     no    |         |                                                                                         |
| `departments`          | [Department](broken-reference)\[]    | yes     |     no    |     no    |         |                                                                                         |
| `workingHours`         | [Working Hours](broken-reference)\[] |         |     no    |     no    |         |                                                                                         |

### Holiday Object

Holiday Object is represented as simple flat JSON objects with the following keys:

| Name   | Type     | Read-only | Mandatory | Default | Description          |
| ------ | -------- | :-------: | :-------: | :-----: | -------------------- |
| `name` | string   |     no    |    yes    |         | Name of the holiday. |
| `date` | DateTime |     no    |    yes    |         | Date of the holiday. |

### Working Hours Object

Working Hours Object is represented as simple flat JSON objects with the following keys:

| Name           | Type   | Read-only | Mandatory | Default | Description                                                                                |
| -------------- | ------ | :-------: | :-------: | :-----: | ------------------------------------------------------------------------------------------ |
| `dayOfWeek`    | string |     no    |    yes    |         | Including `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday` and `sunday`. |
| `startTime`    | time   |     no    |    yes    |         |                                                                                            |
| `endTime`      | time   |     no    |    yes    |         |                                                                                            |
| `awayStatusId` | Guid   |     no    |     no    |         |                                                                                            |

## Shift Endpoints

### Get all Shifts in site

`GET /api/v3/globalSettings/shifts`

#### Parameters

Query string

| Name      | Type   | Required | Default | Description                             |
| --------- | ------ | -------- | ------- | --------------------------------------- |
| `include` | string | no       |         | Available value: `department`, `agent`. |

#### Response

The response is a list of [Shift](broken-reference) Objects.

#### Example

Using curl

```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/shifts?include=department
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
    "name": "Shifts",
    "timeZone": "(GMT) Coordinated Universal Time",
    "ifAutoDetectDayLight": false,
    "holidays": [{
      "name": "summary",
      "date": "2019-11-11"
    },
    ...
    ],
    "agentIds": [68],
    "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
    "departments": [{// include department
      "id": "1DC43077-E36F-F9EA-C7BA-C29620102F7E",
      "name": "departments",
      "siteId": "FEF12049-BF08-2CD4-C405-A5FC8AE75D0F",
      "description": "departments",
      "isAvailableInChat": false,
      "isAvailableInTicketingAndMessaging": false,
      "offlineMessageMailTo": "The email address(es)",
      "offlineMessageEmailAddresses": "test@comm100.com",
      "agentId": 68,
      ...
      },
      ...,
    ],
    "workingHours": [{
      "dayOfWeek": "sunday",
      "startTime": "12:00",
      "endTime": "14:00",
      "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
      },
      ...
    ]
  },
  ...
]
```

### Get a Shift by id

`GET /api/v3/livechat/shifts/{id}`

#### Parameters

Path Parameters

| Name | Type | Required | Description                     |
| ---- | ---- | -------- | ------------------------------- |
| `id` | Guid | yes      | Unique identifier of the shift. |

Query string

| Name      | Type   | Required | Default | Description                             |
| --------- | ------ | -------- | ------- | --------------------------------------- |
| `include` | string | no       |         | Available value: `department`, `agent`. |

#### Response

The response is a [Shift](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED?include=department
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
  "name": "Shifts",
  "timeZone": "(GMT) Coordinated Universal Time",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "departments": [{// include department
    "id": "1DC43077-E36F-F9EA-C7BA-C29620102F7E",
    "name": "departments",
    "siteId": "FEF12049-BF08-2CD4-C405-A5FC8AE75D0F",
    "description": "departments",
    "isAvailableInChat": false,
    "isAvailableInTicketingAndMessaging": false,
    "offlineMessageMailTo": "The email address(es)",
    "offlineMessageEmailAddresses": "test@comm100.com",
    "agentId": 68,
    ...
    },
    ...
  ],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

### Get all Shifts by department id

`GET /api/v3/globalSettings/departments/{departmentId}/shifts`

#### Parameters

Path Parameters

| Name           | Type | Required | Description                   |
| -------------- | ---- | -------- | ----------------------------- |
| `departmentId` | Guid | yes      | Identifier of the department. |

#### Response

The response is a [Shift](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/departments/1DC43077-E36F-F9EA-C7BA-C29620102F7E/shifts
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
    "name": "Shifts",
    "timeZone": "(GMT) Coordinated Universal Time",
    "ifAutoDetectDayLight": false,
    "holidays": [{
      "name": "summary",
      "date": "2019-11-11"
    },
    ...
    ],
    "agentIds": [68],
    "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
    "workingHours": [{
      "dayOfWeek": "sunday",
      "startTime": "12:00",
      "endTime": "14:00",
      "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
    ]
  },
  ...
]
```

### Get all Shifts by agent id

`GET /api/v3/globalSettings/agents/{agentId}/shifts`

#### Parameters

Path Parameters

| Name      | Type    | Required | Description                     |
| --------- | ------- | -------- | ------------------------------- |
| `agentId` | integer | yes      | Unique identifier of the agent. |

#### Response

The response is a [Shift](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agents/68/shifts
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json

[
  {
    "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
    "name": "Shifts",
    "timeZone": "(GMT) Coordinated Universal Time",
    "ifAutoDetectDayLight": false,
    "holidays": [{
      "name": "summary",
      "date": "2019-11-11"
    },
    ...
    ],
    "agentIds": [68],
    "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
    "workingHours": [{
      "dayOfWeek": "sunday",
      "startTime": "12:00",
      "endTime": "14:00",
      "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
    ]
  },
  ...
]
```

### Create a new Shift

`POST /api/v3/globalSettings/shifts`

#### Parameters

Request Body

The request body contains data with the [Shift](broken-reference) structure.

example:

```
{
  "name": "Shifts1111",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

#### Response

The response is a [Shift](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
  "name": "Shifts1111",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/shifts
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED

{
  "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
  "name": "Shifts",
  "timeZone": "(GMT) Coordinated Universal Time",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

### Update a Shift

`PUT /api/v3/globalSettings/shifts/{id}`

#### Parameters

Path Parameters

| Name | Type | Required | Description                     |
| ---- | ---- | -------- | ------------------------------- |
| `id` | Guid | yes      | Unique identifier of the shift. |

Request Body

The request body contains data with the [Shift](broken-reference) structure.

example:

```
{
  "name": "Shifts2222",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

#### Response

The response is a [Shift](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
  "name": "Shifts2222",
  "timeZone": "UTC",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
  }' -X PUT https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json

{
  "id": "3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED",
  "name": "Shifts2222",
  "timeZone": "(GMT) Coordinated Universal Time",
  "ifAutoDetectDayLight": false,
  "holidays": [{
    "name": "summary",
    "date": "2019-11-11"
  },
  ...
  ],
  "agentIds": [68],
  "departmentIds": ["1DC43077-E36F-F9EA-C7BA-C29620102F7E"],
  "workingHours": [{
    "dayOfWeek": "sunday",
    "startTime": "12:00",
    "endTime": "14:00",
    "awayStatusId": "BAACB779-2E41-27C5-B23D-1C8F2058862D"
    },
    ...
  ]
}
```

### Delete a Shift

`DELETE /api/v3/globalSettings/shifts/{id}`

#### Parameters

Path Parameters

| Name | Type | Required | Description                     |
| ---- | ---- | -------- | ------------------------------- |
| `id` | Guid | yes      | Unique identifier of the shift. |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/shifts/3964B5AE-6DAD-D774-BFCB-8C1F6B58ACED
```

Response

```
  HTTP/1.1 204 No Content
```

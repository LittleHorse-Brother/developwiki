# Permission

Permission is hard-coded.

* `GET /api/v3/globalSettings/permissions` - [Get all permissions](broken-reference)
* `GET /api/v3/globalSettings/roles/{roleId}/permissions` - [Get role permissions](broken-reference)
* `GET /api/v3/globalSettings/agents/{agentId}/permissions` - [Get agent permissions](broken-reference)
* `GET /api/v3/globalSettings/agents/{agentId}/permissions:effective` - [Get a list of agent's effective permissions](broken-reference)
* `PUT /api/v3/globalSettings/roles/{roleId}/permissions` - [Update role permissions](broken-reference)
* `PUT /api/v3/globalSettings/agents/{agentId}/permissions` - [Update agent permissions](broken-reference)

## Permission Related Object JSON format

### Permission Object

| Name          | Type   | Include | Read-only | Mandatory | Default | Description                                                                                     |
| ------------- | ------ | :-----: | :-------: | :-------: | :-----: | ----------------------------------------------------------------------------------------------- |
| `id`          | long   |         |     no    |     no    |         |                                                                                                 |
| `name`        | string |         |    yes    |     no    |         |                                                                                                 |
| `description` | string |         |    yes    |     no    |         |                                                                                                 |
| `category`    | string |         |    yes    |     no    |         | The value options include: liveChat, ticketingAndMessaging, bot, globalSettings, knowledgeBase. |

## Permission Endpoints

### Get all permission

`GET /api/v3/globalSettings/permissions`

#### Parameters

```
No parameters
```

#### Response

The response is a list of [Permission](broken-reference) Objects

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/permissions
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json[
{
  "id": 201,
  "name": "Accept Chats",
  "description": "Accept Chats",
  "category": "Live Chat",
},
...,
]
```

### Get role permissions

`GET /api/v3/globalSettings/roles/{roleId}/permissions`

#### Parameters

Path parameters

| Name     | Type | Required | Description             |
| -------- | ---- | -------- | ----------------------- |
| `roleId` | Guid | yes      | Identifier of the role. |

#### Response

The response is a [Permission](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d68927777/permissions
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json[
  {
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

### Get agent permissions

`GET /api/v3/globalSettings/agents/{agentId}/permissions`

#### Parameters

Path parameters

| Name      | Type    | Required | Description              |
| --------- | ------- | -------- | ------------------------ |
| `agentId` | integer | yes      | Identifier of the agent. |

#### Response

The response is a list of [Permission](broken-reference) Objects

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json[
  {
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

### Get agent effective permissions

`GET /api/v3/globalSettings/agents/{agentId}/permissions:effective`

#### Parameters

Path parameters

| Name      | Type    | Required | Description              |
| --------- | ------- | -------- | ------------------------ |
| `agentId` | integer | yes      | Identifier of the agent. |

#### Response

The response is a list of [Permission](broken-reference) Objects.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions:effective
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json[
  {
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

### update role permissions

`PUT /api/v3/globalSettings/roles/{roleId}/permissions`

#### Parameters

Path parameters

| Name     | Type | Required | Description             |
| -------- | ---- | -------- | ----------------------- |
| `roleId` | Guid | yes      | Identifier of the role. |

Request body

The request body contains data with the [Permission](broken-reference) Id list

example:

```json
[
  201,
  205,
  ...,
]
```

#### Response

The response is a role [Permission](broken-reference) Object list.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '[
  201,
  205,
  ...,
]' -X PUT https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d68927777/permissions
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d68927777/permissions
[{
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

### update agent permissions

`PUT /api/v3/globalSettings/agents/{agentId}/permissions`

#### Parameters

Path parameters

| Name      | Type    | Required | Description              |
| --------- | ------- | -------- | ------------------------ |
| `agentId` | integer | yes      | Identifier of the agent. |

Request body

The request body contains data with the [Permission](broken-reference) Id list

example:

```json
[
  201,
  205,
  ...,
]
```

#### Response

The response is: agent [Permission](broken-reference) Object list.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '[
  201,
  205,
  ...,
]' -X PUT https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/68/permissions
[{
    "id": 201,
    "name": "Accept Chats",
    "description": "Accept Chats",
    "category": "Live Chat",
  },
  ...,
]
```

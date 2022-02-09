# Agent

You need the `Manage Agent & Agent Roles` permission to manage agents.

* `GET /api/v3/globalSettings/agents` - [Get a list of agents in site](broken-reference)
* `GET /api/v3/globalSettings/agents/{id}` - [Get an agent by id](broken-reference)
* `GET /api/v3/globalSettings/roles/{roleId}/agents` - [Get a list of agents by role id](broken-reference)
* `GET /api/v3/globalSettings/departments/{departmentId}/agents` - [Get a list of agents by department id](broken-reference)
* `GET /api/v3/globalSettings/agents/me` - [Get current agent](broken-reference)
* `POST /api/v3/globalSettings/agents` - [Create a new agent](broken-reference)
* `POST /api/v3/globalSettings/agents/{id}:unlock` - [Unlock the agent](broken-reference)
* `POST /api/v3/globalSettings/agents/{id}:changePassword` - [Admin set an agent's password](broken-reference)
* `POST /api/v3/globalSettings/agents/me:changePassword` - [Change own password](broken-reference)
* `PUT /api/v3/globalSettings/agents/{id}` - [Update agent info](broken-reference)
* `PUT /api/v3/globalSettings/agents/me` - [Update current agent info](broken-reference)
* `DELETE /api/v3/globalSettings/agents/{id}` - [Delete an agent](broken-reference)

## Agent Related Object JSON format

### Agent Object

This is the entity details for Agents.

| Name             | Type                              | Include | Read-only | Mandatory |        Default        | Description                                                                                                                                                                                                        |
| ---------------- | --------------------------------- | :-----: | :-------: | :-------: | :-------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `id`             | integer                           |         |    yes    |     no    |                       |                                                                                                                                                                                                                    |
| `email`          | string                            |         |    yes    |    yes    |                       | Agent login email address. Cannot be changed.                                                                                                                                                                      |
| `displayName`    | string                            |         |     no    |     no    |                       | Different Agents can have the same Display Name. If not offered, will set by first name.                                                                                                                           |
| `firstName`      | string                            |         |     no    |    yes    |                       | The first name of the agent.                                                                                                                                                                                       |
| `lastName`       | string                            |         |     no    |    yes    |                       | The last name of agent.                                                                                                                                                                                            |
| `isAdmin`        | bool                              |         |     no    |     no    |         false         | Whether the agent is an administrator or not.                                                                                                                                                                      |
| `isActive`       | bool                              |         |     no    |     no    |          true         | Whether the agent is active or not.                                                                                                                                                                                |
| `phone`          | string                            |         |     no    |     no    |                       | Mobile phone number of the agent.                                                                                                                                                                                  |
| `title`          | string                            |         |     no    |     no    |                       | The title of the agent.                                                                                                                                                                                            |
| `bio`            | string                            |         |     no    |     no    |                       | The bio info of the agent.                                                                                                                                                                                         |
| `timeZone`       | string                            |         |     no    |     no    |                       | Time zone of the agent. Value includes all [Time Zone Option](broken-reference) identifiers, if not offered, will use the site time zone.                                                                          |
| `datetimeFormat` | string                            |         |     no    |     no    | 'MM-dd-yyyy HH:mm:ss' | Date & Time format selected by agents to display on the site. Value options include : MM-dd-yyy HH:mm:ss, MM/dd/yyyy HH:mm:ss, dd-MM-yyyy HH:mm:ss, dd/MM/yyyy HH:mm:ss, yyyy-MM-dd HH:mm:ss, yyyy/MM/dd HH:mm:ss. |
| `createdTime`    | DateTime                          |         |     no    |     no    |          UTC          | The create time of the agent.                                                                                                                                                                                      |
| `isLocked`       | bool                              |         |    yes    |     no    |         false         | Account will be locked after several failed login attempts.                                                                                                                                                        |
| `lockedTime`     | DateTime                          |         |     no    |     no    |          UTC          | When the agent was locked.                                                                                                                                                                                         |
| `lastLoginTime`  | DateTime                          |         |     no    |     no    |          UTC          | The time of the last login to Comm100 account (Control Panel or Agent Console).                                                                                                                                    |
| `permissionIds`  | integer\[]                        |         |     no    |     no    |          NULL         | The list of permission identifiers.                                                                                                                                                                                |
| `permissions`    | [Permission](broken-reference)\[] |   yes   |     no    |     no    |                       | Agent permission settings.                                                                                                                                                                                         |
| `roleIds`        | Guid\[]                           |         |     no    |     no    |          NULL         | The list of the role identifiers, which the agent belongs to. If not offered, will use role identifier of "All Agents" as default.                                                                                 |
| `roles`          | Role\[]                           |   yes   |     no    |     no    |                       | The list of the roles, which the agent belongs to.                                                                                                                                                                 |
| `departmentIds`  | Guid\[]                           |         |     no    |     no    |          NULL         | The list of the department identifiers, which the agent belongs to.                                                                                                                                                |
| `departments`    | [Department](broken-reference)\[] |   yes   |     no    |     no    |                       | The list of the roles, which the agent belongs to.                                                                                                                                                                 |
| `shiftIds`       | Guid\[]                           |         |     no    |     no    |          NULL         | The list of the shift identifiers, which the agent belongs to.                                                                                                                                                     |
| `shifts`         | [Shift](broken-reference)\[]      |   yes   |     no    |     no    |                       | The list of shifts which the agent belongs to.                                                                                                                                                                     |

### Agent List Response Object

Agent List Object for agent list Response, include count and page information.

| Name           | Type     | Include | Read-only | Mandatory | Default | Description                         |
| -------------- | -------- | :-----: | :-------: | :-------: | :-----: | ----------------------------------- |
| `count`        | integer  |    no   |    yes    |     no    |         | The total count of queries.         |
| `nextPage`     | string   |    no   |    yes    |     no    |         | The next page url of the query.     |
| `previousPage` | string   |    no   |    yes    |     no    |         | The previous page url of the query. |
| `agents`       | Agent\[] |    no   |    yes    |     no    |         | A list of agents.                   |

## Agent Endpoints

### Get a list of agents in site

`GET/api/v3/globalSettings/agents`

#### Parameters

Query string

| Name        | Type    | Required | Default                                                  | Description                                                 |
| ----------- | ------- | -------- | -------------------------------------------------------- | ----------------------------------------------------------- |
| `include`   | string  | no       | Available value:`department`,`role`,`permission`,`shift` |                                                             |
| `keywords`  | string  | no       |                                                          | Filter by keywords in agent display name and email address. |
| `pageIndex` | integer | no       | 1                                                        | The page index of the query.                                |
| `pageSize`  | integer | no       | 10                                                       | The page size of the query.                                 |

#### Response

The response is an [Agent List Response ](broken-reference)Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/agents?pageIndex=2",
    "previousPage": "",
    "agents": [{
        "id": 68,
        "email": "Tom@gmail.com",
        "displayName":"Tom",
        "firstName":"Tom",
        "lastName":"Green",
        "title":"CEO",
        ...
    },
    ...
    ]
}
```

### Get an agent by id

* `GET /api/v3/globalSettings/agents/{id}`

#### Parameters

Path parameters

| Name | Type    | Required | Description              |
| ---- | ------- | -------- | ------------------------ |
| `id` | integer | yes      | Identifier of the agent. |

Query string

| Name      | Type   | Required | Default                                                  | Description |
| --------- | ------ | -------- | -------------------------------------------------------- | ----------- |
| `include` | string | no       | Available value:`department`,`role`,`permission`,`shift` |             |

#### Response

The response is an [Agent](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/agents/68?include=role,department
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    ...
}
```

### Get a list of agents by role id

`GET /api/v3/globalSettings/roles/{roleId}/agents`

#### Parameters

Path parameters

| Name     | Type | Required | Description             |
| -------- | ---- | -------- | ----------------------- |
| `roleId` | Guid | yes      | Identifier of the role. |

Query string

| Name        | Type    | Required | Default                                                  | Description                  |
| ----------- | ------- | -------- | -------------------------------------------------------- | ---------------------------- |
| `include`   | string  | no       | Available value:`department`,`role`,`permission`,`shift` |                              |
| `pageIndex` | integer | no       | 1                                                        | The page index of the query. |
| `pageSize`  | integer | no       | 10                                                       | The page size of the query.  |

#### Response

The response is an [Agent List Response](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6/agents
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6/agents?pageIndex=2",
    "previousPage": "",
    "agents": [{
        "id": 68,
        "email": "Tom@gmail.com",
        "displayName":"Tom",
        "firstName":"Tom",
        "lastName":"Green",
        "title":"CEO",
        ...
    },
    ...
    ]
}
```

### Get a list of agents by department id

`GET /api/v3/globalSettings/departments/{departmentId}/agents`

#### Parameters

Path parameters

| Name           | Type | Required | Description                   |
| -------------- | ---- | -------- | ----------------------------- |
| `departmentId` | Guid | yes      | Identifier of the department. |

Query string

| Name        | Type    | Required | Default                                                   | Description                  |
| ----------- | ------- | -------- | --------------------------------------------------------- | ---------------------------- |
| `include`   | string  | no       | Available value:`department`,`role`,`permission`,`shift`. |                              |
| `pageIndex` | integer | no       | 1                                                         | The page index of the query. |
| `pageSize`  | integer | no       | 10                                                        | The page size of the query.  |

#### Response

The response is an [Agent List Response](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/departments/42dwdaww-92e6-4487-a2e8-92e68d68a2e8/agents
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "count": 1234,
    "nextPage": "https://api1.comm100.io/api/v3/globalSettings/departments/42dwdaww-92e6-4487-a2e8-92e68d68a2e8/agents?pageIndex=2",
    "previousPage": "",
    "agents": [{
        "id": 68,
        "email": "Tom@gmail.com",
        "displayName":"Tom",
        "firstName":"Tom",
        "lastName":"Green",
        "title":"CEO",
        ...
    },
    ...
    ]
}
```

### Get current agent

* `GET /api/v3/globalSettings/agents/me`

#### Parameters

Query string

| Name      | Type   | Required | Default                                                   | Description |
| --------- | ------ | -------- | --------------------------------------------------------- | ----------- |
| `include` | string | no       | Available value:`department`,`role`,`permission`,`shift`. |             |

#### Response

The response is an [Agent](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json"
-X GET https://api1.comm100.io/api/v3/globalSettings/agents/me?include=role,department
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    ...
}
```

### Create a new agent

`POST /api/v3/globalSettings/agents`

#### Parameters

Request body

The request body contains data with the [Agent](broken-reference) structure.

example:

```json
{
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

#### Response

The response is: [Agent](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
} ' -X POST https://api1.comm100.io/api/v3/globalSettings/agents
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/68
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

### Unlock the agent

`POST /api/v3/globalSettings/agents/{id}:unlock`

#### Parameters

Path parameters

| Name | Type    | Required | Description              |
| ---- | ------- | -------- | ------------------------ |
| `id` | integer | yes      | Identifier of the agent. |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{} ' -X POST https://api1.comm100.io/api/v3/globalSettings/agents/68:unlock
```

Response

```json
HTTP/1.1 204 No Content
```

### Admin set an agent's password

`POST /api/v3/globalSettings/agents/{id}:changePassword`

#### Parameters

Path parameters

| Name | Type    | Required | Description              |
| ---- | ------- | -------- | ------------------------ |
| `id` | integer | yes      | Identifier of the agent. |

Request body

| Name       | Type   | Required | Default | Description                |
| ---------- | ------ | -------- | ------- | -------------------------- |
| `password` | string | yes      |         | New password of the agent. |

example:

```json
{
  "password": "Aa5847lkdsc&d",
}
```

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
    "password": "Aa5847lkdsc&d",
}' -X POST https://api1.comm100.io/api/v3/globalSettings/agents/68:changePassword
```

Response

```json
HTTP/1.1 204 No Content
```

### Change own password

`POST /api/v3/globalSettings/agents/me:changePassword`

#### Parameters

Request body

| Name              | Type   | Required | Default | Description                    |
| ----------------- | ------ | -------- | ------- | ------------------------------ |
| `currentPassword` | string | yes      |         | Current password of the agent. |
| `newPassword`     | string | yes      |         | New password of the agent.     |

example:

```json
{
  "currentPassword": "Aa2541554",
  "newPassword": "Aa5847lkdsc&d",
}
```

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d ' {
    "currentPassword": "Aa2541554",
    "newPassword": "Aa5847lkdsc&d",
  }' -X POST https://api1.comm100.io/api/v3/globalSettings/agents/me:changePassword
```

Response

```json
HTTP/1.1 204 No Content
```

### Update agent info

`PUT /api/v3/globalSettings/agents/{id}`

#### Parameters

Path parameters

| Name | Type    | Required | Description              |
| ---- | ------- | -------- | ------------------------ |
| `id` | integer | yes      | Identifier of the agent. |

Request body

The request body contains data with the [Agent](broken-reference) structure.

example:

```json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

#### Response

The response is: [Agent](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
} ' -X PUT https://api1.comm100.io/api/v3/globalSettings/agents/68
```

Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/68
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

### Update current agent info

`PUT /api/v3/globalSettings/agents/me`

#### Parameters

Request body

The request body contains data with the [Agent](broken-reference) structure.

example:

```json
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

#### Response

The response is: [Agent](broken-reference) Object.

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d '{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
} ' -X PUT https://api1.comm100.io/api/v3/globalSettings/me
```

Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
Location: https://api1.comm100.io/api/v3/globalSettings/agents/me
{
    "id": 68,
    "email": "Tom@gmail.com",
    "displayName":"Tom",
    "firstName":"Tom",
    "lastName":"Green",
    "title":"CEO",
    "roleIds": [
      "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      ...,
    ]
    ...,
}
```

### Delete an agent

`DELETE /api/v3/globalSettings/agents/{id}`

#### Parameters

Path parameters

| Name | Type    | Required | Description              |
| ---- | ------- | -------- | ------------------------ |
| `id` | integer | yes      | Identifier of the agent. |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/agents/68
```

Response

```json
HTTP/1.1 204 No Content
```

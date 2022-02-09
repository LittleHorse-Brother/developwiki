# Role

You need the `Manage Agent & Agent Roles` permission to manage roles.

* `GET /api/v3/globalSettings/roles` - [Get a list of roles in site](broken-reference)
* `GET /api/v3/globalSettings/roles/{id}` - [Get a role by id](broken-reference)
* `POST /api/v3/globalSettings/roles` - [Create a new role](broken-reference)
* `PUT /api/v3/globalSettings/roles/{id}` - [Update a role](broken-reference)
* `DELETE /api/v3/globalSettings/roles/{id}` - [Delete a role](broken-reference)

## Role Related Object JSON format

### Role Object

| Name            | Type                              | Include | Read-only | Mandatory | Default | Description                                                                                                       |
| --------------- | --------------------------------- | :-----: | :-------: | :-------: | :-----: | ----------------------------------------------------------------------------------------------------------------- |
| `id`            | Guid                              |         |    yes    |     no    |         |                                                                                                                   |
| `name`          | string                            |         |     no    |    yes    |         | Name.                                                                                                             |
| `description`   | string                            |         |     no    |     no    |         | Description of this role.                                                                                         |
| `type`          | string                            |         |    yes    |     no    |  custom | Options: administrator, agent, custom; administrator and agent are the system role types. They cannot be deleted. |
| `agentIds`      | int\[]                            |         |     no    |     no    |   NULL  | The selected agents for this role.                                                                                |
| `agents`        | [Agent](broken-reference)\[]      |   yes   |     no    |     no    |         | The selected agents for this role.                                                                                |
| `permissionIds` | int\[]                            |         |     no    |     no    |   NULL  | The list of permission identifiers assigned to this role.                                                         |
| `permissions`   | [Permission](broken-reference)\[] |   yes   |     no    |     no    |         | Permissions assigned to this role.                                                                                |

## Role Endpoints

### Get a list of roles in site

`GET /api/v3/globalSettings/roles`

#### Parameters

Query string

| Name      | Type   | Required | Default                              | Description |
| --------- | ------ | -------- | ------------------------------------ | ----------- |
| `include` | string | no       | Available value:`agent`,`permission` |             |

#### Response

The response is a list of [Role](broken-reference) objects

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles?include=permission
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
[{
  "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
  "name": "markting",
  "description": "yyyy-MM-dd hh:mm:ss",
  "type": "custom",
  "agentIds":  [
    68,
    ...,
  ],
  "agents": [],
  "permissionIds" :
    [
      201,
      205,
      ...,
    ],
    "permissions": []
},
...,
]
```

### Get a role by id

`GET /api/v3/globalSettings/roles/{id}`

#### Parameters

Path parameters

| Name | Type | Required | Description             |
| ---- | ---- | -------- | ----------------------- |
| `id` | Guid | yes      | Identifier of the role. |

Query string

| Name      | Type   | Required | Default                               | Description |
| --------- | ------ | -------- | ------------------------------------- | ----------- |
| `include` | string | no       | Available value:`agent`,`permission`. |             |

#### Response

The response is a [Role](broken-reference) object

#### Example

Using curl

```
curl -H "Content-Type: application/json" -X GET https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```

Response

```json
HTTP/1.1 200 OK
Content-Type:  application/json
{
  "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
  "name": "markting",
  "description": "yyyy-MM-dd hh:mm:ss",
  "type": "custom",
  "agentIds":  [
    68,
    ...,
  ],
  "agents": [],
  "permissionIds" :
    [
      201,
      205,
      ...,
    ],
    "permissions": []
}
```

### Create a new role

`POST /api/v3/globalSettings/roles`

#### Parameters

Request body

The request body contains data with the [Role](broken-reference) structure.

example:

```json
 {
      "Name": "markting",
      "Description": "yyyy-MM-dd hh:mm:ss",
      "Type": "custom",
      "agentIds":  [
        68,
        ...,
      ],
      "agents": [],
      "permissionIds" :
        [
         201,
         205,
         ...,
        ],
      "permissions": []
    }
```

#### Response

The response is a [Role](broken-reference) object

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d ' {
      "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
      "Name": "markting",
      "Description": "yyyy-MM-dd hh:mm:ss",
      "Type": "custom",
      "agentIds":  [
        68,
        ...,
      ],
      "agents": [],
      "permissionIds" :
        [
         201,
         205,
         ...,
        ],
      "permissions": []
    }' -X POST https://api1.comm100.io/api/v3/globalSettings/roles
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
 {
    "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
    "name": "markting",
    "description": "yyyy-MM-dd hh:mm:ss",
    "type": "custom",
    "agentIds":  [
      68,
      ...,
    ],
    "agents": [],
    "permissionIds" :
      [
        201,
        205,
        ...,
      ],
    "permissions": []
  }
```

### Update a role

`PUT /api/v3/globalSettings/roles/{id}`

#### Parameters

Path parameters

| Name | Type | Required | Description             |
| ---- | ---- | -------- | ----------------------- |
| `id` | Guid | yes      | Identifier of the role. |

Request body

The request body contains data with the [Role](broken-reference) structure.

example:

```json
 {
    "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
    "name": "markting",
    "description": "yyyy-MM-dd hh:mm:ss",
    "type": "custom",
    "agentIds":  [
      68,
      ...,
    ],
    "agents": [],
    "permissionIds" :
      [
        201,
        205,
        ...,
      ],
    "permissions": []
  }
```

#### Response

The response is a [Role](broken-reference) object

#### Example

Using curl

```
curl -H "Content-Type: application/json" -d ' {
      "name": "markting",
      "description": "yyyy-MM-dd hh:mm:ss",
      "type": "custom",
      "agentIds":  [
        68,
        ...,
      ],
      "agents": [],
      "permissionIds" :
      [
        201,
        205,
        ...,
      ],
      "permissions": []
    } ' -X PUT https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```

Response

```json
HTTP/1.1 201 Created
Content-Type:  application/json
Location: https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
{
  "id": "4487fc9d-92e6-4487-a2e8-92e68d6892e6",
  "name": "markting",
  "description": "yyyy-MM-dd hh:mm:ss",
  "type": "custom",
  "agentIds":  [
    68,
    ...,
  ],
  "agents": [],
  "permissionIds" :
    [
      201,
      205,
      ...,
    ],
  "permissions": []
}
```

### Delete a role

`DELETE /api/v3/globalSettings/roles/{id}`

#### Parameters

Path parameters

| Name | Type | Required | Description             |
| ---- | ---- | -------- | ----------------------- |
| `id` | Guid | yes      | Identifier of the role. |

#### Response

HTTP/1.1 204 No Content

#### Example

Using curl

```
curl -X DELETE https://api1.comm100.io/api/v3/globalSettings/roles/4487fc9d-92e6-4487-a2e8-92e68d6892e6
```

Response

```json
HTTP/1.1 204 No Content
```

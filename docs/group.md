# Overview
Group templates must be created with the Cortado MDM in order to be accessed via the API.

Use the access token obtained as described [here](auth.md) on every request as the json field *token*. All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.

**Base API URL: https://go.mycortado.com/api/mdm/v2/group**

### Example Request

```json
POST /api/mdm/v2/group/example HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    ... all other request parameters ...
}
```

### Example Response

| HTTP Status | Description |
| ------------ | ------------ |
| 200 | Request successfull. The response body will contain optional group template data. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |
| 403 | Action forbidden. For example when a user access token is send in the request that is only usable with an admin access token. |

## Group Fields

The following fields can be returned by the API containing information about the user.

| Field | Description |
| ------------ | ------------ |
| **description** | the description of the group template |
| **id** | the id of the group template |
| **name** | the name of the group template |
| **priority** | the priority of the group template |
| **sid** | the sid of the group template |

## Get Group Templates
This request can only be used by sending an admin access token.<br>
By sending such a token informations about all group templates of the admin access tokens tenant can be retrieved.

### Get Group Templates Request

#### Parameters

```json
POST /api/mdm/v2/group/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}"
}
```

### Get Group Templates Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": null,
    "errormessage": null,
    "success": true,
    "tokenstatus": null,
    "data": [
        {
            "description": "This template is used for ccs users that are not in a managed active directory group.",
            "id": 1,
            "name": "Default Group Template",
            "priority": 0,
            "sid": "default_user_template"
        },
        {
            "description": "Description",
            "id": 2,
            "name": "Test 1",
            "priority": 1,
            "sid": "b966e92c-13c6-4bb5-afc2-dbb2a079e697"
        },
        {
            "description": "",
            "id": 3,
            "name": "Test 2",
            "priority": 2,
            "sid": "49e962f0-d188-4100-9d68-43d6191157f3"
        }
    ],
    "pagecount": 1,
    "pageindex": 1,
    "totalcount": 3
}
```
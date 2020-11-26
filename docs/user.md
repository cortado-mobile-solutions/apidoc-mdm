# Overview
Users must be created with the Cortado MDM in order to be accessed via the API.

Use the access token obtained as described [here](auth.md) on every request as the json field *token*. All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.

**Base API URL: https://go.mycortado.com/api/mdm/v2/user**

### Response

| HTTP Status | Description |
| ------------ | ------------ |
| 200 | Request successfull. The response body will contain optional user data. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |
| 404 | The requested entity (e.g. user) was not found. A wrong identifier (e.g. usersid) was passed within the request. |

## User Fields

The following fields can be returned by the API containing information about the user.

| Field | Description |
| ------------ | ------------ |
| **displayname** | display name of the user |
| **email** | email address of the user and logon name |
| **enabled** | *true*, if user is enabled. Otherwise, false. |
| **firstname** | the user's firstname |
| **lastname** | the user's lastname |
| **managedappleid** | the managed Apple ID of the user |
| **phone** | phone number of the user |
| **sid** | user id |

### User Info

#### Parameters

```json
POST /api/mdm/v2/user HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "sid":"{usersid}"
}
```

### User Info Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,
    "userinfo": {
        "displayname": "Firstname Lastname",
        "email": "firstname.lastname@domain.com",
        "enabled": true,
        "firstname": "Firstname",
        "lastname": "Lastname",
        "managedappleid": "firstname.lastname@appleid.domain.com",
        "phone": "123456789",
        "sid": "2640ee91-1cbc-4647-85df-a3daff5055e1"
    }
    }
```
# Overview
This section describes the authentication process with the Cortado MDM API. In order to perform any API requests, an **access token** is required to authenticate any calls.

## Authentication Request
An access token can be issued for **admin or user access**. A user token can only be used for managing devices of the authenticated user. An admin token grants access to all devices of the managed tenant.
An admin account is able to login to the Cortado MDM management console [here](https://go.mycortado.com/fw). A user account is not able to login to this console. A user account can only be created from within the console by the admin.

### Parameters

| Parameter | Values | Description |
| ------------ | ------------- | ------------ |
| **type** | basic | Currently, only basic authentication is supported. |
| **usertype** | user, admin | *user* for user authentication or *admin* for an admin authentication |
| **username** |  | the Cortado MDM user/admin e-mail address |
| **password** |  | the Cortado MDM password of the admin/user |
| **mtcid** |  | the id of your Cortado MDM tenant (required for admin authentication request only) |

```json
POST /api/mdm/v2/user/login HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "type":"basic",
    "usertype":"user|admin",
    "username":"{username}",
    "password":"{password}",
    "mtcid":"{tenant id}"
}
```

## Authentication Response
The server will **always respond with a 200 OK** HTTP status. The *success* field within the response indicates a successful request only if the value is *true*. A failed request will return a response with a detailed error message within the *errormessage* field.

### Fields

| Field | Description |
| ------------ | ------------ |
| **errorcode** | contains an error code, only if *success* is *false*. |
| **errormessage**  | contains an error message, only if *success* is *false*. |
| **success**  | is *true*, if the request is successfull. Otherwise *false*. |
| **tokenstatus**  | is empty, if token is still valid for use. *ExpiresSoon*, if token should be refreshed. *Expired*, if token has expired and a new token needs to be requested. |
| **token** | contains the access token, if *success* is *true*. |

```json
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,
    "token":"{access token}"
}
```

The returned access *token* needs to be included in all subsequent calls, to authenticate with the API.


## Token Renewal/Refresh

If the server responds with **ExpiresSoon**, the current token can be used to request a new token. The server will respond with an Authentication Response.

```json
POST /api/mdm/v2/user/renewtoken HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}"
}
```
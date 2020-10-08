# Overview

This section describes the authentication process with the Cortado MDM API. In order to get access to the API, you need to obtain an **access token** first. An access token can be issued for **admin or user access**.

A user token can only be used for managing devices of the authenticated user. An admin token grants access to all devices of the managed tenant.

### Authentication

### Authentication Request

| First Header | Second Header | Third Header |
| ------------ | ------------- | ------------ |
| Content Cell | Content Cell  | Content Cell |
| Content Cell | Content Cell  | Content Cell |

#### Parameters
* **type**: is always *basic*
* **usertype**: *user* for user authentication or *admin* for an admin authentication
* **username**: the Cortado MDM user/admin e-mail address
* **password**: the Cortado MDM password of the admin/user
* **mtcid**: the unique id of your Cortado MDM tenant (required for admin authentication request only)

```json
POST /ccrest/publicapi/v2/user/login HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "type":"basic",
    "usertype":"user|admin",
    "username":"",
    "password":""
}
```

### Authentication Response

The server will **always respond with a 200 OK** HTTP status. The *success* field within the response indicates a successful request only if the value is *true*. A failed request will return a response with a detailed error message within the *errormessage* field.

#### Fields
* **errorcode**: an error code, if *success* is *false*.
* **errormessage**: an error message, if *success* is *false*.
* **success**: is *true*, if the request is successfull. Otherwise *false*.
* **tokenstatus**: empty, if token is still valid for use. *ExpiresSoon*, if token should be refreshed. *Expired*, if token has expired and a new token needs to be requested.
* **token**: access token, if *success* is *true*.

```json
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,
    "token":"token"
}
```
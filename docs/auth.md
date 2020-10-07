# Overview

This section describes the authentication process with the Cortado MDM API. In order to get access to the API, you need to obtain an **access token** first. An access token can be issued for **admin or user access**.

A user token can only be used for managing devices of the authenticated user. An admin token grants access to all devices of the managed tenant.

## Parameters
* **username**: the Cortado MDM user/admin e-mail address
* **password**: the Cortado MDM password of the admin/user
* **mtcid**: the unique id of your Cortado MDM tenant (required for admin authentication request only)

### User Authentication Request

```json
POST /ccrest/publicapi/v2/user/login HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "type": "basic",
    "usertype": "user",
    "username": "",
    "password": ""
}
```

### Admin Authentication Request

```json
POST /ccrest/publicapi/v2/user/login HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "type": "basic",
    "usertype": "admin",
    "mtcid":"",
    "username": "",
    "password": ""
}
```

### Authentication Response

The server will **always respond with a 200 OK** HTTP status. The *success* field within the response indicates a successful request only if the value is *true*. A failed request will return a response with a detailed error message within the *errormessage* field.

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
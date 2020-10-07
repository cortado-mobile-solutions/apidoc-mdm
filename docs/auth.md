# Authentication

This is how to authenticte with Cortado MDM.


### User Authentication
This request can only be done by endusers (not administrators) that have an Cortado MDM user account set up.
The issued authentication token grants access to the tenant, the user is configured at.

```json
POST /ccrest/publicapi/v2/user/login HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "type": "basic",
    "usertype": "user",
    "username": "%username%",
    "password": "%password%"
}
```

Response

```json
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,"token":"%token%"
}
```
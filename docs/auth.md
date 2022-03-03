# Overview
This section describes the authentication process with the Cortado MDM API. In order to perform any API requests, an **api key authorization header** (recommended) or **access token** is required to authenticate any calls.<br><br>
Using the Accept-Language request header the response localization can be set. Default is "en" for english, possible other value is "de" for german

## Api key authentication
The api key authentication is only available for admin users and all requests that require authentication.<br> An api key can be set for each admin individually through the administrators aspect of the administration portal if the customers plan includes the role-based rights management for administrators.
A detailed description on how to set the api key can be found [here](https://support.cortado.com/en/support/solutions/articles/43000660252-how-to-access-the-cortado-mdm-api)
To authenticate using an api key an authorization header is required. Please note that a access token will always outrank an api key, if both authentication methods are send within the same request

The authorization header has to use the following structure...

*Authorization: Api-Key my_api_key*

Additionally a **mtcid** parameter has to be added to the requests body if the admin user has access to multiple tenants. This parameter has to contain the id of the Cortado MDM tenant for which the request should be executed. It is of course mandatory that the api key administrator has access to this tenant

### Parameters

| Parameter | Values | Description |
| ------------ | ------------- | ------------ |
| **mtcid** |  | The id of the Cortado MDM tenant. The mtcid for a tenant can be found in the settings aspect (general tab) of the administration portal.<br> This parameter is optional if the admin user of the api key only has access to one tenant, but mandatory if he has access to multiple tenants. |

```json
POST /api/mdm/v2/user/example HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
	...
    "mtcid":"{tenant id}",
	...
}
```

## Authentication Request
**This authentication method should only be used to execute requests in a user context. For the admin context the api key authentication is recommended**<br>
An access token can be issued for **admin or user access**. A user token can only be used for managing devices of the authenticated user. An admin token grants access to all devices of the managed tenant.<br>
An admin account is able to login to the Cortado MDM management console [here](https://go.mycortado.com/fw). A user account is not able to login to this console. A user account can only be created from within the console by the admin.

### Parameters

| Parameter | Values | Description |
| ------------ | ------------- | ------------ |
| **type** | basic | Currently, only basic authentication is supported. |
| **usertype** | user, admin | *user* for user authentication or *admin* for an admin authentication |
| **username** |  | The Cortado MDM user/admin e-mail address |
| **password** |  | The Cortado MDM password of the admin/user |
| **mtcid** |  | The id of your Cortado MDM tenant (required for admin authentication request only). The mtcid for a tenant can currently only be retrieved by a Cortado MDM master account |

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

### Fields

| Field | Description |
| ------------ | ------------ |
| **errorcode** | Ccontains an error code, only if *success* is *false*. |
| **errormessage**  | Contains an error message, only if *success* is *false*. |
| **success**  | Is *true*, if the request is successfull. Otherwise *false*. |
| **tokenstatus**  | Is empty, if token is still valid for use. *ExpiresSoon*, if token should be refreshed. *Expired*, if token has expired and a new token needs to be requested. |
| **token** | Contains the access token, if *success* is *true*. |

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
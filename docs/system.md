# Overview
Use the access token obtained as described [here](auth.md) on every request where the json field *token* is required. All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.

**Base API URL: https://go.mycortado.com/api/mdm/v2/system**

### Example Request

```json
POST /api/mdm/v2/system/example HTTP/1.1
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
| 200 | Request successfull. The response body will contain optional data about the system. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |

## System Fields

The following fields can be returned by the API containing information about the system.

| Field | Description |
| ------------ | ------------ |
| **mtcenabled** | *true*, if MTC (Multi Tenacy Mode) is enabled, otherwise *false* |
| **showairprint** | *true*, if the airprint section is displayed in the user portal, otherwise *false* |
| **showapps** | *true*, if the apps section is displayed in the user portal, otherwise *false* |
| **showdeviceenrollment** | *true*, if the device enrollment section is displayed in the user portal, otherwise *false* |
| **showsharepoint** | *true*, if the sharepoint section is displayed in the user portal, otherwise *false* |

## System Info
Retrieve informations about the system. This request does not need any authentication and the response will contain informations about the system in the *systeminfo* section.

### System Info Request

#### Parameters

```json
POST /api/mdm/v2/system/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
}
```

### System Info Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
   "errorcode": null,
   "errormessage": null,
   "success": true,
   "tokenstatus": null,
   "systeminfo": {
      "mtcenabled": true
   }
}
```
## Download Root CA
Retrieve the Root CA public key part.

### Download Root CA Request

#### Parameters

```json
POST /api/mdm/v2/system/rootca HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}"
}
```

This request is also available as a GET request. Just pass the json as a query parameters in this case.
```
(...)/system/rootca?json={"token":"x"}
```

### System Info Response
The Root CA as a file
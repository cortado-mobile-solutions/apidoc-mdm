# Overview
Use the header *cms-dhsc* with the value *true* or *1* if the response http code should always be 200. In this case the *success* field within the response indicates a successful request only if the value is *true*. A failed request will return a response with a detailed error message within the *errormessage* field.<br>
**Recommended:**To authenticate use the api key, obtained as described [here](auth.md), on every request as the authorization header like this: *Authorization: Api-Key my_api_key*<br>
To execute requests in a user context use the access token, obtained as described [here](auth.md), on the request as the json field *token*.<br>
All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.<br>
Using the Accept-Language request header the response localization can be set. Default is "en" for english, possible other value is "de" for german

**Base API URL: https://go.mycortado.com/api/mdm/v2/system**

### Example Request

```json
POST /api/mdm/v2/system/example HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    ... all request parameters ...
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
| **userportal - enabled** | *true*, if the user portal is enabled, otherwise *false* |
| **showairprint** | *true*, if the airprint section is displayed in the user portal, otherwise *false* |
| **showapps** | *true*, if the apps section is displayed in the user portal, otherwise *false* |
| **showdeviceenrollment** | *true*, if the device enrollment section is displayed in the user portal, otherwise *false* |
| **showsharepoint** | *true*, if the sharepoint section is displayed in the user portal, otherwise *false* |
| **wdienabled** | *true*, if the wdi client is enabled, otherwise *false* |

## System Info
Retrieve informations about the system. This request does not need any authentication and the response will contain informations about the system in the *systeminfo* section.

### System Info Request

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
	  ...
   }
}
```
## Download Root CA
Retrieve the Root CA public key part.

### Download Root CA Request

```json
POST /api/mdm/v2/system/rootca HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
}
```

This request is also available as a GET request. Just pass the json as a query parameter in this case. (url encoding of the parameter might be necessary)
```
(...)/system/rootca?json={"token":"x"}
```

### System Info Response
The Root CA public key part as a file
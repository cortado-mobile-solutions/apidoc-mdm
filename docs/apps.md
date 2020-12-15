# Overview
Apps must be successfully assigned to a user or a device in order to be accessed via the API.

Use the access token obtained as described [here](auth.md) on every request as the json field *token*. All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.

**Base API URL: https://go.mycortado.com/api/mdm/v2/app**

### Example Request

```json
POST /api/mdm/v2/app/example HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "clientid": "{client_id}",
    "token":"{access token}",
    ... all other request parameters ...
}
```

### Example Response

| HTTP Status | Description |
| ------------ | ------------ |
| 200 | Request successfull. The response body will contain optional app data. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |
| 404 | The requested entity (e.g. device) was not found. A wrong identifier (e.g. clientid) was passed within the request. |

## App Fields

The following fields can be returned by the API containing information about the apps.

| Field | Description |
| ------------ | ------------ |
| **actions** | Clarify with mipas |
| **apptype** | Clarify with mipas |
| **compatibletodevice** | *true*, if the app is compatible to the optional passed device identified through the clientid, otherwise *false* |
| **devicetype** | Indicated if the app is compatible to smartphones and/or tablets |
| **displayname** | The name of the app displayed in the management console |
| **id** | The id of the app |
| **managed** | *true*, if the app is managed, otherwise *false* |
| **mandatory** | *true*, if the app is mandatory, otherwise *false* |
| **platform** | The platform of the app, "Apple" or "Android" |
| **status** | The status of the app. Values can be\n0: We do not have a status for the app, so it is maybe not installed, or we cannot see this until someone tries to install.\n1: Installing or uninstalling the app. Status details maybe contains a constant for describing the state in detail.\n2: An error has occured. Status details maybe contains a constant for describing the state in detail.\n3: App is installed and managed. |
| **statusdetails** | Details about the status of the app |

**Important**
Getting the expected value for the status field could take some time, because the app installation might need user interaction (depending on the enrollment type) and an update of the status is triggered through the device sync or manually by the admin.

## Get App List
Retrieve a list of all apps assigned to a user and a optional device. The response contains an array of all apps assigned to the authenticated user and the device (if a clientid was send). Using an administrator access token to request the apps, the response will contain all apps of the managed tenant.

### Get App List Request

#### Parameters
The *clientid* is optional

```json
POST /api/mdm/v2/app/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "clientid": "{client_id}",
    "token":"{access token}"
}
```

### Get App List Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": null,
    "errormessage": null,
    "success": true,
    "tokenstatus": "ExpiresSoon",
    "data": [
        {
            "actions": {
                "install": {
                    "visible": false
                },
                "uninstall": {
                    "visible": false
                }
            },
            "apptype": "AU",
            "compatibletodevice": false,
            "devicetype": "SmartphoneAndTablet",
            "displayname": "1Password - Password Manager and Secure Wallet by AgileBits",
            "id": "AU8",
            "managed": true,
            "mandatory": true,
            "platform": "Android",
            "status": 0,
            "statusdetails": null
        },
        {
            "actions": {
                "install": {
                    "visible": true
                },
                "uninstall": {
                    "visible": false
                }
            },
            "apptype": "AU",
            "compatibletodevice": true,
            "devicetype": "SmartphoneAndTablet",
            "displayname": "OpenVPN Connect",
            "id": "AU210",
            "managed": true,
            "mandatory": false,
            "platform": "Apple",
            "status": 0,
            "statusdetails": null
        }
    ],
    "pagecount": 1,
    "pageindex": 1,
    "totalcount": 2
}
```
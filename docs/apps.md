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
| **actions** | This sections contains actions that can, or cannot, (indicated by a boolean value) be carried out on the app. Evaluating this part only makes sense if the clientid was send in the request. |
| **apptype** | The type of the app. Possible values are<br>AU: App Url<br>EA: Enterprise App<br>MU: Multimedia Url  |
| **compatibletodevice** | *true*, if the app is compatible to the optional passed device identified through the clientid, otherwise *false* |
| **devicetype** | Indicates if the app is compatible to smartphones and/or tablets |
| **displayname** | The name of the app displayed in the management console |
| **id** | The id of the app |
| **managed** | *true*, if the app is managed, otherwise *false* |
| **mandatory** | *true*, if the app is mandatory, otherwise *false* |
| **platform** | The platform of the app, "Apple" or "Android" |
| **status** | The status of the app. Values can be<br>0: We do not have a status for the app, so it is maybe not installed, or we cannot see this until someone tries to install.<br>1: Installing or uninstalling the app. Status details maybe contains a constant for describing the state in detail.<br>2: An error has occured. Status details maybe contains a constant for describing the state in detail.<br>3: App is installed and managed. |
| **statusdetails** | Details about the status of the app |

**Important**
Getting the expected value for the status field could take some time, because the app installation might need user interaction (depending on the enrollment type) and an update of the status is triggered through the device sync or manually by the admin.

## Get App List
Retrieve a list of all apps assigned to a user and a optional device. The response contains an array of all apps assigned to the authenticated user and the device (if a clientid was send). Using an administrator access token to request the apps, the response will contain all apps of the managed tenant.

### Get App List Request

#### Parameters
The *clientid* is optional. It can be retrieved through the device list request as described [here](device.md)

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

## Get App Info
Retrieve informations about an app that is assigned to a user or a device (with clientid parameter) or to a user (without clientid parameter). The response contains informations about the app assigned to the authenticated user and/or the device. Using an administrator access token to request the app info, the response will contain informations about the app of the managed tenant.

### Get App Info Request

#### Parameters
The *appid* can be obtained through the Get App List request
The *clientid* is optional. It can be retrieved through the device list request as described [here](device.md)

```json
POST /api/mdm/v2/app/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
	"id": "{app_id}",
    "clientid": "{client_id}",
    "token":"{access token}"
}
```

### Get App Info Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": null,
    "errormessage": null,
    "success": true,
    "tokenstatus": null,
    "data": {
        "actions": {
            "install": {
                "visible": false
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
        "status": 1,
        "statusdetails": "WaitingForDeviceReply"
    }
}
```

## Get App Image
Retrieve the image of an app. The response is the image file.

### Get App Image Request

#### Parameters
The *appid* can be obtained through the Get App List request

```json
POST /api/mdm/v2/app/image HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
	"id": "{app_id}",
    "token":"{access token}"
}
```

This request is also available as a GET request. Just pass the json as a query parameters in this case.
```
(...)app/image?json={"id": "x","token": "x"}
```

### Get App Image Response
The image file

## Install App
With this request an app installation for a device of a user can be triggered. The app has to be already assigned to the user or the device and it has to be compatible. The response contains informations about the success or failure of the installation . Using an administrator access token the app can be pushed to any device.
The request can be used for optional and mandatory apps.

### Install App Request

#### Parameters
The *appid* can be obtained through the Get App List request
The *clientid* can be retrieved through the device list request as described [here](device.md)

```json
POST /api/mdm/v2/app/install HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
	"id": "{app_id}",
    "clientid": "{client_id}",
    "token":"{access token}"
}
```

### Install App Success Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": null,
    "errormessage": null,
    "success": true,
    "tokenstatus": null
}
```

### Install App Error Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": "02200021",
    "errormessage": "App with id 'AU8' is not compatible to device with client id 'dc1685473f6448dfbb86dc8348cec2d7'.",
    "success": false,
    "tokenstatus": "ExpiresSoon"
}
```

## Uninstall App
With this request an app uninstallation for a device of a user can be triggered. The app has to be already assigned to the user or the device and it has to be compatible. The response contains informations about the success or failure of the uninstallation . Using an administrator access token the app can be uninstalled from any device.
The request can be used for optional and mandatory apps.

### Uninstall App Request

#### Parameters
The *appid* can be obtained through the Get App List request
The *clientid* can be retrieved through the device list request as described [here](device.md)

```json
POST /api/mdm/v2/app/uninstall HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
	"id": "{app_id}",
    "clientid": "{client_id}",
    "token":"{access token}"
}
```

### Uninstall App Success Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": null,
    "errormessage": null,
    "success": true,
    "tokenstatus": null
}
```

### Uninstall App Error Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode": "02200021",
    "errormessage": "App with id 'AU8' is not compatible to device with client id 'dc1685473f6448dfbb86dc8348cec2d7'.",
    "success": false,
    "tokenstatus": "ExpiresSoon"
}
```
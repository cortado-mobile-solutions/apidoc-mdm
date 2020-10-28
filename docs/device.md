# Overview
Devices must be successfully enrolled with the Cortado MDM in order to be accessed via the API.

Use the access token obtained as described [here](auth.md) on every request as the json field *token*. All additional request parameters are also added as json fields to th request body. The request content type must be *application/json*.

**Base API URL: https://go.mycortado.com/api/v2**

### Example Request

```json
POST /api/v2/device/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid": "{client_id}",
    ... all other request parameters ...
}
```

### Request Responses

| HTTP Status | Description |
| ------------ | ------------ |
| 200 | Request successfull. The response body will contain optional device data. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |
| 404 | The requested entity (e.g. device) was not found. A wrong identifier (e.g. clientid) was passed within the request. |


```json
POST /api/v2/device/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid": "{client_id}",
    ... all other request parameters ...
}
```

## Device Fields

The following fields can be returned by the API containing information about the device.

| Field | Description |
| ------------ | ------------ |
| **clientid** | unique device identifier |
| **imei** | device IMEI (if device has a SIM card inserted) |
| **serialnumber** | device serial number |
| **modelname** | device model name |
| **displayname** | device display name |
| **lastcontact** | date and time of last contact / check-in of the device |
| **location** | last known location incl. timestamp of the device |
| **lostmodeenabled** | *true*, if the lost-mode is enabled for the device. Otherwise, *false*. |
| **passwordenabled** | *true*, if the screen lock on the device is configured (Android only). |
| **recoverytoken** | if the lost mode is enabled on the device it contains a recovery token that can be used to manually disable the lost mode on the device (Android only).|
| **supervised** | *true*, if the device is an iOS/iPad OS device and is supervised. Otherwise, *empty* or *false*. |
| **androidforworktype**  | Android device management mode. 2: Work Profile, 3: Fully Managed |
| **enrollmenttype** | shows the enrollment type of the device. Values can be 0: Zero-Touch (Android), 1: Device Enrollment Program (iOS/ipadOS), 2: User Enrollment (iOS/ipadOS), 3: Samsung Knox Mobile Enrollment. If the device was not enrolled with one the listed above then *null* indcates no enrollment method. |
| **managed** | *true*, if the device is managed by the MDM. Otherwise, *false*. |
| **type** | indicates the operating system of the device. 2: iOS/ipadOS, 4: Android, 7: macOS |


## List Devices
Retrieve a list of all managed devices and basic details about the device status. The response contains an array of all devices assigned to the authenticated user. Using an administrator access token to request the devices, the response will contain all devices of the managed tenant.

### List Devices Request

#### Parameters

```json
POST /api/v2 HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}"
}
```

### List Devices Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,
    "devices":
        [
            {
                "clientid":"6f53a61b42754fd790cfdf12dfa5ab45",
                "displayname":"My iPad",
                "imei":null,
                "lastcontact":"\/Date(1675991537000)\/",
                "location":null,
                "lostmodeenabled":false,
                "modelname":"Apple iPad Air 2, Space Gray (MGL12FD)",
                "passwordenabled":false,
                "recoverytoken":null,
                "serialnumber":"DMPEEB445VJ",
                "supervised":true
            }
        ]
    }
```


## Device Info
Returns detailed information about a device.

### Device Info Request

### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device.

```json
POST /api/v2/device/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid":"{client id}"
}
```

### Device Info Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,
    "deviceinfo":
        {
            "androidforworktype":3,
            "clientid":"CIBWMD901DA0947CA9E14F35B0",
            "displayname":"P00A",
            "enrollmenttype":null,
            "imei":null,
            "lastcontact":"\/Date(1601998193433)\/",
            "location":null,
            "lostmodeenabled":false,
            "managed":true,
            "modelname":"Android P00A",
            "passwordenabled":false,
            "recoverytoken":null,
            "serialnumber":"H5NPCX088662FXW",
            "supervised":false,
            "type":4
        }
}
```


## Lock Screen
Locks the screen of the device.

### Lock Screen Request

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device.

| Field | Description |
| ------------ | ------------ |
| **message** | Optional message shown on the lockscreen (iOS only) |
| **phonenumber** | Optional phone number shown on the lockscreen (iOS only)|


```json
POST /api/v2/device/lockscreen HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid":"{client id}",
    "message":"{message}",
    "phonenumber":"{phonenumber}"
}
```

### Lock Screen Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null
}
```

## Wipe
Wipes the device. Depending on the current management mode, the device is reset to factory defaults or only a work container is removed from the device.

### Wipe Request
Use either *clientid*, *imei* or *serialnumber* to specify the device.

```json
POST /api/v2/device/wipe HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid":"{client id}"    
}
```

### Wipe Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null
}
```



## Lost Mode
Depending on the management mode of the device, the lost mode can be enabled on the device to lock down the device. If the lost mode is enabled, the device passcode can be changed and the location can be retrieved.

### Enable Lost Mode

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device.

| Field | Description |
| ------------ | ------------ |
| **message** | message which is shown on the lockscreen of the device, if the lost mode was enabled. |
| **phonenumber** | Phone number (Android only)|
| **footnote** | footnote on the bottom of the lockscreen, if the lost mode was enabled. |
| **password** | new passcode that will be set for the device to unlock (Android only) |

#### Enable Lost Mode Request

```json
POST /api/v2/device/enablelostmode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid":"{client id}",
    "message":"Please call the following number for unlock instructions.",
    "phonenumber":"0123456789",
    "footnote":"This device is managed by Cortado.",
    "password":"1234"
}
```

#### Enable Lost Mode Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null
}
```

### Disable Lost Mode

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device.

#### Disable Lost Mode Request

```json
POST /api/v2/device/disablelostmode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid":"{client id}"
}
```

#### Disable Lost Mode Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null
}
```

## Location
The retrieval of the device location can be triggered. Fetching the latest retrieved device location can be done using the [device status](#device-info) request.

### Trigger Location Retrieval Request

### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device.

```json
POST /api/v2/device/requestlocation HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "clientid":"{client id}"
}
```

### Trigger Location Retrieval Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null
}
```
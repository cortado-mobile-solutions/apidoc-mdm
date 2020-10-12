# Overview
Devices must be successfully enrolled with the Cortado MDM in order to be accessed via the API. 

## Device Fields

| Field | Example Value | Description |
| ------------ | ------------- | ------------ |
| **clientid** | | unique device identifier |
| **imei** | | device IMEI |
| **serialnumber** | | device serial number |
| **modelname** | | device model name |
| **displayname** | | device display name |
| **lastcontact** | | date and time of last contact / check-in of the device |
| **location** | | last known location incl. timestamp of the device |
| **lostmodeenabled** | | *true*, if the lost-mode is enabled for the device. Otherwise, *false*. |
| **passwordenabled** | | |
| **recoverytoken** | | |
| **supervised** | | *true*, if the device is an iOS/iPad OS device and is supervised. Otherwise, *empty* or *false*. |
| **androidforworktype** |  |  |
| **enrollmenttype** | |  |
| **managed** | | |
| **type** | | |


## List Devices
Retrieve a list of all managed devices and basic details about the device status. The response contains an array of all devices assigned to the authenticated user. Using an administrator access token to request the devices, the response will contain all devices of the managed tenant.

### List Devices Request

#### Parameters

```json
POST /api/v2/device/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":""
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
                "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
                "displayname":"My iPad",
                "imei":null,
                "lastcontact":"\/Date(1675991537000)\/",
                "location":null,
                "lostmodeenabled":false,
                "modelname":"Apple iPad Air 2, Space Gray (MGL12FD)",
                "passwordenabled":false,
                "recoverytoken":null,
                "serialnumber":"DMPERB4G5VJ",
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
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "token":""
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
            "actions":
                {
                    "disablelostmode":
                        {
                            "status":0,
                            "visible":false
                        },
                    "enablelostmode":
                        {"
                            status":0,
                            "visible":true
                        },
                    "lockscreen":
                        {
                            "status":0,
                            "visible":true
                        },
                    "requestlocation":
                        {
                            "status":0,
                            "visible":false
                        },
                    "wipefull":
                        {
                            "status":0,
                            "visible":true
                        }
                },
            "androidforworktype":3,
            "clientid":"CIDWMD501DA0947CA9E14F35B0",
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


## Lost Mode
Depending on the management mode of the device, the lost mode can be enabled on the device to lock down the device. If the lost mode is enabled, the device passcode can be changed and the location can be retrieved.

### Enable Lost Mode

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device.

| Field | Example Value | Description |
| ------------ | ------------- | ------------ |
| **message** | | message which is shown on the lockscreen of the device, if the lost mode was enabled. |
| **phonenumber** | | Phone number **(Android only)**|
| **footnote** | | footnote on the bottom of the lockscreen, if the lost mode was enabled. |
| **password** | | new passcode that will be set for the device to unlock **(Android only)** |

#### Enable Lost Mode Request

```json
POST /api/v2/device/enablelostmode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "message":"Message shown on the device lock screen",
    "phonenumber":"Message shown on the device lock screen",
    "footnote":"Message shown on the device lock screen",
    "password":"Message shown on the device lock screen",
    "token":""
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
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "token":""
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
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "token":""
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
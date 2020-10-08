# Overview
Devices must be successfully enrolled with the Cortado MDM in order to be accessed via the API. 

#### Device Fields
* **clientid**: the device identifier
* **imei**: the device IMEI
* **serialnumber**: the device serial number
* **modelname**: device model information
* **displayname**: display name of the device
* **lastcontact**: last contact / check-in of the device with the MDM
* **location**: contains last known location of the device
* **lostmodeenabled**: is *true*, if lost the lost-mode is enabled. Otherwise false
* **passwordenabled**: 
* **recoverytoken**: 
* **supervised**: 

* **androidforworktype**: 3
* **enrollmenttype**: null
* **managed**: true
* **type**: 4


## List Devices
Retrieve a list of managed devices and basic details about the device status. The response lists all devices assigned to the authenticated user. Using an administrator access token to request the devices, the response will contain all devices of the managed tenant.

### List Devices Request

#### Parameters
* **token**: access token obtained during [authentication](/en/latest/auth)

```json
POST /ccrest/publicapi/v2/device/list HTTP/1.1
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
* **clientid**: 
* **imei**: 
* **serialnumber**: 
* **token**: access token obtained during [authentication](/en/latest/auth)

```json
POST /ccrest/publicapi/v2/device/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "imei":"",
    "serialnumber":"",
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
* **clientid**: 
* **message**: 
* **phonenumber**: 
* **footnote**: 
* **password**: only for Android!
* **token**: access token obtained during [authentication](/en/latest/auth)

#### Enable Lost Mode Request

```json
POST /ccrest/publicapi/v2/device/enablelostmode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "message":"Message shown on the device lock screen",
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
* **clientid**: 
* **token**: access token obtained during [authentication](/en/latest/auth)

#### Disable Lost Mode Request

```json
POST /ccrest/publicapi/v2/device/disablelostmode HTTP/1.1
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
* **clientid**: 
* **token**: access token obtained during [authentication](/en/latest/auth)

```json
POST /ccrest/publicapi/v2/device/requestlocation HTTP/1.1
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
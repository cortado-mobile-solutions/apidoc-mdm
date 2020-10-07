# Overview
Devices must be successfully enrolled with the Cortado MDM in order to be accessed via the API. 


## List Devices
Retrieve a list of managed devices and basic details about the device status. The response lists all devices assigned to the authenticated user. Using an administrator access token to request the devices, the response will contain all devices of the managed tenant.

### List Devices Request

### Parameters
* **token**: access token obtained during [authentication](/auth.md)

```json
POST /ccrest/publicapi/v2/device/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":""
}
```

### List Devices Response

#### Parameters
* **clientid**: 
* **displayname**: 
* **imei**: 
* **lastcontact**: 
* **location**: 
* **lostmodeenabled**: 
* **modelname**: 
* **passwordenabled**: 
* **recoverytoken**: 
* **serialnumber**: 
* **supervised**: 


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


## Lost Mode
Depending on the management mode of the device, the lost mode can be enabled on the device to lock down the device. If the lost mode is enabled, the device passcode can be changed and the location can be retrieved.


### Enabled Lost Mode

#### Parameters
* **clientid**: 
* **message**: 
* **token**: 

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
* **token**: 

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

## Retrieve Location
The retrieval of the device location can be triggered. Fetching the latest retrieved location data is done using the device status request.

### Retrieve Location Request

### Parameters
* **clientid**: 
* **token**: 

```json
POST /ccrest/publicapi/v2/device/requestlocation HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "clientid":"6f53a61b42764fd690cfdf12dfa5ab45",
    "token":""
}
```

### Retrieve Location Response

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
# Device
Devices must be successfully enrolled with the Cortado MDM in order to be accessed via the API. 


## List Devices

### List Devices Request

### Parameters
* **token**: access token obtained during [authentication](/auth)

```json
POST /ccrest/publicapi/v2/device/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":""
}
```

### List Devices Response
The response lists all devices assigned to the authenticated user. Using an administrator access token to request the devices, the response will contain all devices of the managed tenant.

## Parameters
* **clientid**: 
* **displayname**: 
* **imei**: 
* **lastcontact**: 
* **location**: 
* **lostmodeenabled**: 
* **modelname**: 
* **passwordenabled**: 
* **recoverytoken**: 
* **recoverytoken**: 
* **supervised**: 


```json
POST /ccrest/publicapi/v2/device/list HTTP/1.1
Host: go.mycortado.com
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

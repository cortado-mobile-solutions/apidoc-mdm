# Overview
Devices must be successfully enrolled with the Cortado MDM in order to be accessed via the API.

**Recommended:**To authenticate use the api key, obtained as described [here](auth.md), on every request as the authorization header like this: *Authorization: Api-Key my_api_key*<br>
To execute requests in a user context use the access token, obtained as described [here](auth.md), on the request as the json field *token*.<br>
Use either the clientid, imei or serialnumber of the device, which can be retrieved through the device list request, on every request except the device list request as the json fields *clientid*, *imei* or *serialnumber*<br>
All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.<br>
Using the Accept-Language request header the response localization can be set. Default is "en" for english, possible other value is "de" for german

**Base API URL: https://go.mycortado.com/api/mdm/v2/device**

### Example Request with clientid

```json
POST /api/mdm/v2/device/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid": "{client_id}",
    ... all other request parameters ...
}
```

### Example Request with imei

```json
POST /api/mdm/v2/device/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "imei": "{device_imei}",
    ... all other request parameters ...
}
```

### Example Request with serialnumber

```json
POST /api/mdm/v2/device/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "serialnumber": "{device_serialnumber}",
    ... all other request parameters ...
}
```

### Example Response

| HTTP Status | Description |
| ------------ | ------------ |
| 200 | Request successfull. The response body will contain optional device data. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |
| 404 | The requested entity (e.g. device) was not found. A wrong identifier (e.g. clientid) was passed within the request. |

## Device Fields

The following fields can be returned by the API containing information about the device.

| Field | Description |
| ------------ | ------------ |
| **actions** | This sections contains actions that can, or cannot, be carried out on the device.<br>When the status parameter is "1" the action is already pending.<br> The visible parameter indicates if the action is available and can be displayed |
| **androidforworktype**  | Android device management mode.<br>2: Work Profile<br>3: Fully Managed<br>4: Work Profile on Company Owner Device |
| **batterylevel** | Battery level in percent |
| **clientid** | Unique device identifier |
| **displayname** | Device display name |
| **enrollmenttype** | Shows the enrollment type of the device. Values can be<br>0: Zero-Touch (Android)<br>1: Device Enrollment Program (macOS only)<br>2: User Enrollment (iOS/ipadOS)<br>3: Samsung Knox Mobile Enrollment. If the device was not enrolled with one of the listed above then *null* indicates no enrollment method. |
| **exchangeid** | Exchange id of the device |
| **freestorageinfo** | Free storage on the device |
| **freestoragemb** | Free storage on the device in megabytes |
| **imei** | Device IMEI (if device has a SIM card inserted) |
| **lastcontact** | Date and time of last contact / check-in of the device |
| **location** | Last known location incl. timestamp of the device |
| **lostmodeenabled** | *true*, if the lost mode is enabled for the device. Otherwise, *false*. |
| **managed** | *true*, if the device is managed by the MDM. Otherwise, *false*. |
| **mdmid** | On iOS supervised: UDID<br>On iOS User Enrollment: Enrollment ID<br>On Android: Android ID |
| **mdmprofileremoved** | *true* if the mdm profile was removed from the device |
| **modelname** | Device model name |
| **osname** | Display name of the installed OS |
| **passcodecompliant** | *true*, id the device is compliant to the password policy, otherwise *false* |
| **passwordenabled** | *true*, if the screen lock on the device is configured (Android only), otherwise *false* |
| **recoverytoken** | If the lost mode is enabled on the device it contains a recovery token that can be used to manually disable the lost mode on the device (Android only).|
| **roaming** | Indicates, if the device is roaming |
| **serialnumber** | Device serial number |
| **storagemb** | Total storage of the device |
| **supervised** | *true*, if the device is an iOS/iPad OS device and is supervised. Otherwise, *empty* or *false*. |
| **type** | Indicates the operating system of the device.<br>2: iOS/ipadOS<br>4: Android<br>7: macOS |
| **usersid** | The user id of the associated user (details on the user can be retrieved with a [user request](user.md)) |
| **wiped** | *true* if the device was wiped, otherwise *false* |


## List Devices
Retrieve a list of all managed devices and basic details about the device status. The response contains an array of all devices assigned to the authenticated user. Using an administrator access token to request the devices, the response will contain all devices of the managed tenant.<br>
With the filter parameter the devices can be filtered for wiped/managed devices or for devices within a certain last contact or location timestamp timespan. All filters are combinable

### List Devices Request

```json
POST /api/mdm/v2/device/list HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "filter": {
        "managed": true,
        "wiped": true,
        "lastcontact": {
            "from": "2025-01-06",
            "to": "2025-08-08"
        },
        "locationtimestamp": {
            "from": "2025-01-06",
            "to": "2025-08-08"
        }
    }
}
```

#### Parameters
| Field | Description |
| ------------ | ------------ |
| **filter** | Optional filter parameter. |
| **filter - managed** | Optional boolean parameter. Set to *true* to only retrieve managed devices, *false* for not managed devices. |
| **filter - wiped** | Optional boolean parameter. Set to *true* to only retrieve wiped devices, *false* for not wiped devices. |
| **filter - lastcontact (from/to)** | Optional DateTime string parameters. Can be used to only retrieve devices with a last contact value within a certain timespan. (including) |
| **filter - locationtimestamp (from/to)** | Optional DateTime string parameters. Can be used to only retrieve devices with a location timestamp value within a certain timespan. (including). A device with no location timestamp won´t be returned if this filter is used |

#### Examples for supported DateTime values in filter
```text
2024-12-01
2024-12-01T00:00:00.000Z
2024-12-01T07:00:00.000+7
2024-12-01T07:00:00.000+07
2024-12-01T07:00:00.000+07:00
2024-12-01T00:00:00Z
2024-12-01T07:00:00+7
2024-12-01T07:00:00+07
2024-12-01T07:00:00+07:00
20241201T000000Z
```


### List Devices Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
   "errorcode": null,
   "errormessage": null,
   "success": true,
   "tokenstatus": null,
   "data": [
      {
         "actions": {
            "disablelostmode": {
               "status": 0,
               "visible": false
            },
            "enablelostmode": {
               "status": 0,
               "visible": true
            },
            "lockscreen": {
               "status": 0,
               "visible": true
            },
            "requestlocation": {
               "status": 0,
               "visible": false
            },
            "resetpasscode": {
               "status": 0,
               "visible": true
            },
            "wipefull": {
               "status": 0,
               "visible": true
            },
			"wipepartial": {
			"status": 0,
			"visible": false
			}			
         },
         "androidforworktype": 0,
         "batterylevel": 95,
         "clientid": "e60cf0e25e63430694441870c27a8b42",
         "displayname": "iPhone",
         "enrollmenttype": null,
		 "exchangeid": null,
         "freestorageinfo": "52.2 GB / 64.0 GB - 81.55 %",
         "freestoragemb": 53444,
         "imei": "357351093108963",
         "lastcontact": "/Date(1608564096937)/",
         "location": null,
         "lostmodeenabled": false,
         "managed": true,
         "mdmid": "00008020-001E04DA2284002E",
		 "mdmprofileremoved": false,
         "modelname": "Apple iPhone XR, Blue (MRYA2ZD)",
         "osname": "iOS 14.2",
         "passwordenabled": true,
         "recoverytoken": null,
         "roaming": false,
         "serialnumber": "F71XL2HZKXK7",
         "storagemb": 65536,
         "supervised": true,
         "type": 2,
         "usersid": "465874fd-28a6-4f0d-bc53-43df50552fe2",
		 "wiped": false
      }
   ],
   "pagecount": 1,
   "pageindex": 1,
   "totalcount": 1
}
```


## Device Info
Returns detailed informations about a user device. Using an administrator access token the device info of any device of the tenant can be retrieved.

### Device Info Request

```json
POST /api/mdm/v2/device/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request

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
         "actions": {
            "disablelostmode": {
               "status": 0,
               "visible": false
            },
            "enablelostmode": {
               "status": 0,
               "visible": true
            },
            "lockscreen": {
               "status": 0,
               "visible": true
            },
            "requestlocation": {
               "status": 0,
               "visible": false
            },
            "resetpasscode": {
               "status": 0,
               "visible": true
            },
            "wipefull": {
               "status": 0,
               "visible": true
            },
			"wipepartial": {
			"status": 0,
			"visible": false
			}			
         },
         "androidforworktype": 0,
         "batterylevel": 95,
         "clientid": "e60cf0e25e63430694441870c27a8b42",
         "displayname": "iPhone",
         "enrollmenttype": null,
		 "exchangeid": null,
         "freestorageinfo": "52.2 GB / 64.0 GB - 81.55 %",
         "freestoragemb": 53444,
         "imei": "357351093108963",
         "lastcontact": "/Date(1608564096937)/",
         "location": null,
         "lostmodeenabled": false,
         "managed": true,
         "mdmid": "00008020-001E04DA2284002E",
		 "mdmprofileremoved": false,
         "modelname": "Apple iPhone XR, Blue (MRYA2ZD)",
         "osname": "iOS 14.2",
         "passwordenabled": true,
         "recoverytoken": null,
         "roaming": false,
         "serialnumber": "F71XL2HZKXK7",
         "storagemb": 65536,
         "supervised": true,
         "type": 2,
         "usersid": "465874fd-28a6-4f0d-bc53-43df50552fe2",
		 "wiped": false
      }
}
```


## Lock Screen
Locks the screen of the user device. Using an administrator access token the screen of any device of the tenant can be locked.

### Lock Screen Request

```json
POST /api/mdm/v2/device/lockscreen HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}",
    "message":"{message}",
    "phonenumber":"{phonenumber}",
	"macpin":"{macpin}"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request

| Field | Description |
| ------------ | ------------ |
| **message** | Optional message shown on the lockscreen (iOS only) |
| **phonenumber** | Optional phone number shown on the lockscreen (iOS only)|
| **macpin** | Optional 6-digit pin (macOS only)|

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

## (Partial)Wipe
(Partial)Wipes the user device. Depending on the current management mode, the device is reset to factory defaults or only a work container is removed from the device. Using an administrator access token any device of the tenant can be wiped.<br>
Sending the optional parameter *partial* with the value *true* a partial wipe will be, if possible, performed. This will only removed the mdm data on the device

### (Partial)Wipe Request

```json
POST /api/mdm/v2/device/wipe HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}",
    "macpin":"{macpin}",
    "partial":"true"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request

| Field | Description |
| ------------ | ------------ |
| **macpin** | Optional 6-digit pin (macOS, full wipe only) |
| **partial** | Optional boolean parameter. Set to *true* to perform a partial wipe |

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



## Enable Lost Mode
Depending on the management mode of the device, the lost mode can be enabled on the device to lock down the device. If the lost mode is enabled, the device passcode can be changed and the location can be retrieved. Using an administrator access token the lost mode of any device of the tenant can be enabled.

### Enable Lost Mode Request

```json
POST /api/mdm/v2/device/enablelostmode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}",
    "message":"Please call the following number for unlock instructions.",
    "phonenumber":"0123456789",
    "footnote":"This device is managed by Cortado.",
    "password":"1234"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request<br>
Sending either the message and/or phonenumber parameter is mandatory

| Field | Description |
| ------------ | ------------ |
| **message** | Message which is shown on the lockscreen of the device, if the lost mode was enabled. |
| **phonenumber** | Phone number which is shown on the lockscreen of the device, if the lost mode was enabled. |
| **footnote** | Footnote on the bottom of the lockscreen, if the lost mode was enabled. |
| **password** | New passcode that will be set for the device to unlock (Android only) |

### Enable Lost Mode Response

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

## Disable Lost Mode
This will disable a possibly enabled lost mode of a device

### Disable Lost Mode Request

```json
POST /api/mdm/v2/device/disablelostmode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request

### Disable Lost Mode Response

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

## Trigger Location Retrieval
The retrieval of the user device location can be triggered. Fetching the latest retrieved device location can be done using the [device info](#device-info) request. Using an administrator access token the retrieval of the location of any device of the tenant can be triggered.

### Trigger Location Retrieval Request

```json
POST /api/mdm/v2/device/requestlocation HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request

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

## Reset Passcode
This will reset the passcode of a device. For Apple devices the passcode will be cleared and, depending on a possible device policy, a new one has to be set. For Android devices the new password has to get send with the request.

### Reset Passcode Request

```json
POST /api/mdm/v2/device/resetpasscode HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "password":"{new password}",	
    "clientid":"{client id}"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request<br>
The password parameter is only for Android devices mandatory and only then evaluated.

| Field | Description |
| ------------ | ------------ |
| **password** | new passcode that will be set for the device to unlock (Android only) |

### Reset Passcode Response

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

## Device Mdm App List
With this request, the device is requested to send back its current apps and docs list to the MDM server. This request will create new MDM cmds (if not already open cmds of the same type exist) for the device and try to reach it via APNS (for Android it will use the Play API). The device will then connect anytime (maybe 3-5 s, or a day, or a week) with the MDM server and try to reply the MDM cmds. After that the new app list data is available and can be get by the AppList or AppInfo request. Normally if everything works fine (device connection, cloud), the data should be there in 3-5 seconds.<br>
Using an administrator access token the retrieval of the location of any device of the tenant can be triggered.

### Device Mdm App List Request

```json
POST /api/mdm/v2/device/requestapplist HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
Authorization: Api-Key my_api_key

{
    "clientid":"{client id}"
}
```

#### Parameters
Use either *clientid*, *imei* or *serialnumber* to specify the device. They can be retrieved through the list devices request

### Device Mdm App List Response

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
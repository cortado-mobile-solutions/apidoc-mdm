# Overview<sup>[1](#myfootnote1)</sup>
Users must be created with the Cortado MDM in order to be accessed via the API.

Use the access token obtained as described [here](auth.md) on every request as the json field *token*. All additional request parameters are also added as json fields to the request body. The request content type must be *application/json*.

**Base API URL: https://go.mycortado.com/api/mdm/v2/user**

### Example Request

```json
POST /api/mdm/v2/user/example HTTP/1.1
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
| 200 | Request successfull. The response body will contain optional user data. |
| 401 | Authentication has failed. The access token was not passed or is invalid. You need to refresh the access token first. |
| 404 | The requested entity (e.g. user) was not found. A wrong identifier (e.g. usersid) was passed within the request. |

## User Fields

The following fields can be returned by the API containing information about the user.

| Field | Description |
| ------------ | ------------ |
| **displayname** | the display name of the user |
| **email** | the email address of the user and logon name |
| **enabled** | *true*, if user is enabled. Otherwise, *false* |
| **firstname** | the firstname of the user |
| **lastname** | the lastname of the user |
| **managedappleid** | the managed Apple ID of the user |
| **phone** | the phone number of the user |
| **sid** | the sid of the user |

## User Info
With this request informations about a user can be retrieved by sending the access token. An admin can request the informations of any user by additionally sending the users sid.<br>
When sending an admin access token and no sid parameter the user informations for the admin will be returned. The "enabled" response parameter will always be "false" in this case

### User Info Request

#### Parameters
The sid parameter is only used if the request is send by an admin. It can be retrieved through the device list request as described [here](device.md)

| Field | Description |
| ------------ | ------------ |
| **sid** | Optional sid of the user |

```json
POST /api/mdm/v2/user/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
    "token":"{access token}",
    "sid":"{usersid}"
}
```

### User Info Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
    "errorcode":null,
    "errormessage":null,
    "success":true,
    "tokenstatus":null,
    "userinfo": {
        "displayname": "Firstname Lastname",
        "email": "firstname.lastname@domain.com",
        "enabled": true,
        "firstname": "Firstname",
        "lastname": "Lastname",
        "managedappleid": "firstname.lastname@appleid.domain.com",
        "phone": "123456789",
        "sid": "2640ee91-1cbc-4647-85df-a3daff5055e1"
    }
}
```

## User Change Password
With this request the password of a user can be changed. This request is only available for MTC(Cloud) installations

### User Change Password Request

#### Parameters

| Field | Description |
| ------------ | ------------ |
| **confirmnewpassword** | The confirmation of the new password |
| **newpassword** | The new password |
| **oldpassword** | The current password |

```json
POST /api/mdm/v2/user/changepassword HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "token": "{access token}",
   "confirmnewpassword": "{new password}",
   "newpassword": "{new password}",
   "oldpassword": "{old password}"
}
```

### User Change Password Response

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

## User Forgot Password
This request will trigger the forgot password email. It is only available for MTC(Cloud) installations<br>
The email will contain a link to reset the password which also contains a reset password token which has to be used in the follow-up request to get the user rest password info or to reset the password

### User Forgot Password Request

#### Parameters
Valid user types are: user, admin

| Field | Description |
| ------------ | ------------ |
| **emailaddress** | The email address of the user |
| **usertype** | The user type (user or admin) |

```json
POST /api/mdm/v2/user/forgotpassword HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "emailaddress": "{email address}",
   "usertype": "{user type}"
}
```

### User Forgot Password Response
The response will always be successful, only because of infrastructural problems (network, database etc.) an error code will be returned<br>
If the user exists an email, with the necessary informations, will get send to the user.

## User Reset Password
With this request the password of a user can be reset. It is only available for MTC(Cloud) installations<br>
It will trigger a confirmation email or an instruction email about enrolling a new device (if the optional parameter *join* was send with the value *true*)

### User Reset Password Request

#### Parameters
Reset password token - This token is part of the forgot password email that is send to a user through the forgot password request.

| Field | Description |
| ------------ | ------------ |
| **confirmnewpassword** | The confirmation of the new password |
| **newpassword** | The new password |
| **join** | Optional boolean value. If this is *true* an email about enrolling a new device will be send to the customer after the successful password reset instead of a confirmation email that the password was successfully changed |

```json
POST /api/mdm/v2/user/resetpassword HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "token": "{reset password token}",
   "confirmnewpassword": "{new password}",
   "newpassword": "{new password}",
   "join": "true"
}
```

### User Reset Password Success Response

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

## User Reset Password Info
With this request informations about a reset password token, like the display name of the user, can be retrieved. These informations will be in the response section *userresetpasswordinfo*. It is only available for MTC(Cloud) installations

### User Reset Password Info Request

#### Parameters
Reset password token - This token is part of the forgot password email that is send to a user through the forgot password request.

```json
POST /api/mdm/v2/user/resetpasswordinfo HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "token": "{reset password token}"
}
```

### User Reset Password Info Response

```json
HTTP/1.1 200 OK
Content-Type: application/json

{
   "errorcode": null,
   "errormessage": null,
   "success": true,
   "tokenstatus": null,
   "userresetpasswordinfo": {
      "displayname": "..."
   }
}
```

## User MDM Profile
With this request a users mdm profile can be downloaded. This request is only avilable for users, not admins, and the response will be a file stream<br>
When using Apple BYOD make sure that the user has a *Managed Apple ID* through the user info request first.

### User MDM Profile Request

#### Parameters

| Field | Description |
| ------------ | ------------ |
| **mdmtype** | The mdmtype. Valid values are "android" or "apple" |
| **byod** | Optional boolean parameter. Set to *true* if a mdm profile for a byod device is requested |
| **mac** | Optional boolean parameter. Set to *true* if a mdm profile for a mac device is requested |
| **html** | Optional boolean parameter. Set to *true* if a response html page should be returned |

##### User MDM Profile Request for Apple iOS/iPadOS BYOD with html response

```json
POST /api/mdm/v2/user/mdmprofile HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "mdmtype": "apple",
   "byod": "true",
   "token": "{access token}",
   "html": "true"
}
```

##### User MDM Profile Request for Apple macOS BYOD

```json
POST /api/mdm/v2/user/mdmprofile HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "mdmtype": "apple",
   "mac": "true",   
   "byod": "true",
   "token": "{access token}"   
}
```

##### User MDM Profile Request for Android

```json
POST /api/mdm/v2/user/mdmprofile HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json

{
   "mdmtype": "android",
   "token": "{access token}"   
}
```

This request is also available as a GET request. Just pass the json as a query parameter in this case. (url encoding of the parameter might be necessary)<br>
To show a web page use "html" with "true".
```
(...)user/mdmprofile?json={"mdmtype":"apple","byod":"true","html":"true","token":"x"}
```

### User MDM Profile Response
File stream

<a name="myfootnote1">1</a>: Use the header *cms-dhsc* with the value *true* or *1* if the response http code should always be 200. In this case the *success* field within the response indicates a successful request only if the value is *true*. A failed request will return a response with a detailed error message within the *errormessage* field.<br>
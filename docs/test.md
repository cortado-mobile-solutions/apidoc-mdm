# Overview

**Base API URL: https://go.mycortado.com/api/mdm/v2/test**

### Example Request

```json
POST /api/mdm/v2/test/example HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
```

### Example Response

| HTTP Status | Description |
| ------------ | ------------ |
| 200 | Request successfull. |

## Hello world
With this request the general availability of the api can be checked

### System Info Request

#### Parameters

```json
POST /api/mdm/v2/system/info HTTP/1.1
Host: go.mycortado.com
Content-Type: application/json
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
   "message":"Hello, world."
}
```
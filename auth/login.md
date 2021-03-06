## Login
Client Login to Blink Account on Blink Servers

`POST /api/v4/account/login`

### Headers
- **Content-Type** -  `application/json`

### Body
- **email** - Account userid/email
- **password** - Account password
- **unique_id** - (optional) UUID generated and identifying the client.  Pass a consistent value here to avoid repeated client verification PIN requests.

### Response
- **account&#46;id** - Account Identifier 
- **client&#46;id** - Client Identifier
- **client.verification_required** - Client verification required by Blink Servers, see [Verify Pin](verifyPin.md).
- **authtoken.authtoken** - String Authentication token to be passed as `TOKEN_AUTH` header in future calls
- **region** - Region Object (see notes below)

### Notes
Depending on where your Blink system is registered, the region object info appears necessary to form the localized URL of the REST endpoint for future calls. Current logic seems to be to update the REST endpoint using region.tier data in the response:
- from `https://rest-prod.immedia-semi.com`
- to `https://rest-{region.tier}.immedia-semi.com` Reports indicate that all regions are not implemented equally, i.e. not all endpoints are available in all regions

### Example Request
```sh
curl 'https://rest-prod.immedia-semi.com/api/v4/account/login' -X POST  -H 'Content-Type: application/json' -d '{"unique_id": "00000000-0000-0000-0000-000000000000", "password":"aPassword","email":"anEmail"}'
```


### Example Response
`200 OK`

```javascript
{
  "account": {
    "id": 1234,
    "verification_required": false,
    "new_account": false
  },
  "client": {
    "id": 1234567,
    "verification_required": true
  },
  "authtoken": {
    "authtoken": "anAuthTokenString",
    "message": "auth"
  },
  "region": {
    "tier": "prod",
    "description": "United States",
    "code": "us"
  },
  "lockout_time_remaining": 0,
  "force_password_reset": false,
  "allow_pin_resend_seconds": 60
}
```

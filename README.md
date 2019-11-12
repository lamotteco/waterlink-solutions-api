# WaterLink Solutions 3rd Party API

### Environment URLS

Environment | Main API | Authentication
---|---|---
Staging | https://api2.staging.waterlinkconnect.com | https://auth2.staging.waterlinkconnect.com
Production | https://wls-api.waterlinkconnect.com | https://wls-auth.waterlinkconnect.com



## Authenticating as a 3rd party application

In order to authenticate your application, you must make a `POST` request to the Authentication end point of `/external-applications/authentication`.  The body of the request will be a JSON payload that contains your unqiue application id and your secret key.  

#### Request body
```json
{
    "applicationId": "unique-app-id",
    "secretKey": "secretKey"
}
```

The response will be a JSON payload that contains a bearer token and an ISO-8601 date that represents the date and time that the related token will expire.  These tokens are not renewable and additional authentication requests must be made after the token expires.

### Respone body
```json
{
    "expiresOn": "2019-11-12T22:17:59.0993119+00:00",
    "bearerToken": "[string-token]"
}
```

## Communicating with the Main API

In order to provide authorization to the Main APIs, all requests should include an `Authorization` header using the `Bearer` scheme with the token returned from the authentication api.

A sample request might look like:

```http
GET /external-applications/mine HTTP/1.1
Host: https://api2.staging.waterlinkconnect.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vYXV0aGVudGljYXRpb24ubGFtb3R0ZS5jb20iLCJ
Accept: */*
```

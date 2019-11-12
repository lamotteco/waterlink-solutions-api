# WaterLink Solutions 3rd Party API

### Environment URLS

API | Staging | Production
---|---|---
Main API | `https://api2.staging.waterlinkconnect.com` | `https://wls-api.waterlinkconnect.com`
Authentication | `https://auth2.staging.waterlinkconnect.com` | `https://wls-auth.waterlinkconnect.com`
User Bridge: | `https://solutions.staging.waterlinkconnect.com/bridge/[bridge-token]` | N/A




## Authenticating as a 3rd party application

In order to authenticate your application, you must make a `POST` request to the Authentication API end point of `/external-applications/authentication`.  The body of the request will be a JSON payload that contains your unqiue application id and your secret key.  

#### Request body
```json
{
    "applicationId": "unique-app-id",
    "secretKey": "secretKey"
}
```

The response will be a JSON payload that contains a bearer token and an ISO-8601 date that represents the date and time that the related token will expire.  These tokens are not renewable and additional authentication requests must be made after the token expires.

#### Respone body
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


## Authenticating application users to the WaterLink Solutions portal

Application users have a portal in the WaterLink Solutions product which allows them to customize their chemicals list and treatment profiles.  This portal also allows them to select from templates which create these chemicals and profiles based on some standard market specific settings.

In order to authenticate an application user, the application should make a `POST` request to the Authentication API end-point: `/external-applications/{application-id}/users/authentication`.  Where `{application-id}` is the unique id for your application.  The request body will be a JSON payload that contains your application unique id, the user's unique id, and your application secret key.

#### Request body
```json
{
    "applicationId": "[app-unique-id]",
    "userId": "[user-unique-id]",
    "secretKey": "[app-secret-key]"
}
```

The response will be a JSON payload that contains a bridge token and an ISO-8601 date and time that the token will expire.  These tokens are not renewable and additional authentication requests must be made after the token expires.

#### Response body
```json
{
    "expiresOn": "2019-11-12T22:17:59.0993119+00:00",
    "bearerToken": "[bridge-token]"
}
```


The bridge token can be passed on the URL to the User Bridge URL to log the user into their portal.
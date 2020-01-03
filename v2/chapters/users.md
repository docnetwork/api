# Users

A user object contains the following fields:

- `email` **[REQUIRED]** The user's email address
- `givenName` **[REQUIRED]** The user's given name
- `familyName` **[REQUIRED]** The user's family name
- `phone` The user's phone number
- `location` The user's mailing address

> Note: Fields may only be edited on user creation. If phone or location data are not included, the user will be required to enter this information when they access their account.

## ROUTES
```
GET    /v2/organizations/{orgID}/profiles/{profileID}/users
POST   /v2/organizations/{orgID}/profiles/{profileID}/users
DELETE /v2/organizations/{orgID}/profiles/{profileID}/users
POST   /v2/organizations/{orgID}/sso/{email}
```

## Get a Profile's Users

Route:

```
GET /v2/organizations/{orgID}/profiles/{profileID}/users
```

Request:

```
GET /v2/organizations/12345/profiles/197715/users HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "users": [
    {
      "email": "hugh@campdoc.com",
      "givenName": "Hugh",
      "familyName": "Honey"
    },
    {
      "email": "vic@campdoc.com",
      "givenName": "Vic",
      "familyName": "Vinegar"
    }
  ]
}
```

## Create a User

> Note: This action grants the created user immediate access to the profile specified in the request URI. User details cannot be edited after creation. If a user exists with the email address in your request, that existing user is granted access to the profile.

Route:

```
POST /v2/organizations/{orgID}/profiles/{profileID}/users
```

Request:

```
POST /v2/organizations/12345/profiles/197715/users HTTP/1.1
Host: app.campdoc.com
Content-Type: application/json

{
  "email": "zoe@campdoc.com",
  "givenName": "Zoe",
  "familyName": "Washburn",
  "phone": "8008675309",
  "location": {
    "addr1": "15 New Sudbury St",
    "city": "Boston",
    "state": "MA",
    "zip": "02203",
    "country": "United States",
    "formatted": "15 New Sudbury St, Boston, MA, 02203, United States"
  }
}
```

Response `201 Created`

## Unlink a profile and user

> This action will deactivate the link between the given profile and user. The user may be re-added to the profile with a standard POST request.

Route:
```
DELETE /v2/organizations/{orgID}/profiles/{profileID}/users
```

Request:
```
DELETE /v2/organizations/12345/profiles/12345/users
Host: app.campdoc.com
Content-Type: application/json

{
  "email": "zoe@campdoc.com"
}
```

Response `204 No Content`

## Generate Sign-On Link (SSO)

> Generate an SSO link to allow the specified user to access their account without entering an email address or password. Links are valid for 60 minutes and may only be used once.

Route:

```
POST /v2/organizations/{orgID}/sso/{email}
```

Request:

```
POST /v2/organizations/12345/sso/zoe@campdoc.com HTTP/1.1
Host: app.campdoc.com
```

Response `201 Created`:

```json
{
  "sso": {
    "url": "https://app.campdoc.com/sso/e139875-7dc9-4c9c-8a96-35df218de8cb",
    "user": {
      "email": "zoe@campdoc.com",
      "givenName": "Zoe",
      "familyName": "Washburn"
    },
    "expires": "2019-02-17T22:02:23.165Z"
  }
}
```

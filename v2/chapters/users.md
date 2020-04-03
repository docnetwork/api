# Users

A user object contains the following fields:

- `email` **[REQUIRED]** The user's email address
- `givenName` **[REQUIRED]** The user's given name
- `familyName` **[REQUIRED]** The user's family name
- `phone` The user's phone number
- `location` The user's mailing address

> Fields may only be edited on user creation. If phone or location data are not included, the user will be required to enter this information when they access their account.

## ROUTES
```
GET    /v2/organizations/{orgID}/profiles/{profileID}/users
POST   /v2/organizations/{orgID}/profiles/{profileID}/users
DELETE /v2/organizations/{orgID}/profiles/{profileID}/users
POST   /v2/organizations/{orgID}/sso/{email}
```

## Get a Profile's Users

```
GET /v2/organizations/{orgID}/profiles/{profileID}/users
```

Accepted Scopes:
```
all:read
all:write
users:read
users:write
```

Request:

```
GET /v2/organizations/12345/profiles/54321/users HTTP/1.1
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

```
POST /v2/organizations/{orgID}/profiles/{profileID}/users
```

> This action grants the created user immediate access to the profile specified in the request URI. User details cannot be edited after creation. If a user exists with the email address in your request, that existing user is granted access to the profile.

Accepted Scopes:
```
all:write
users:write
```

Request:

```
POST /v2/organizations/12345/profiles/54321/users HTTP/1.1
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

> By default, the API will not send any invite or other notification to the created user's email address. To send a notification, include a `?notify=true` query in your request URI. This notification will inform the user they've been given access to a profile, and will include a link to access their account. This link will expire once used, or after 6 months by default. To set a custom expiration on the link, include another query `&inviteLinkExpiration=YYYY-MM-DD HH:mm:ss` in your request URI with a max value of 6 months. You may also pass a date with no time information to expire at midnight on the provided date. All values will be treated as UTC.

## Unlink a profile and user

```
DELETE /v2/organizations/{orgID}/profiles/{profileID}/users
```
> This action will deactivate the link between the given profile and user. The user may be re-added to the profile with a standard POST request.

Accpeted Scopes:
```
all:write
users:write
```

Request:
```
DELETE /v2/organizations/12345/profiles/54321/users
Host: app.campdoc.com
Content-Type: application/json

{
  "email": "zoe@campdoc.com"
}
```

Response `204 No Content`

## Generate Sign-On Link (SSO)

```
POST /v2/organizations/{orgID}/sso/{email}
```

> Generate an SSO link to allow the specified user to access their account without entering an email address or password. Links are valid for 60 minutes and may only be used once.

Accepted Scopes:
```
sso:generate
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

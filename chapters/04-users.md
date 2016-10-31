# Users

A user object contains the following fields.  They are read-only in all cases except when initially creating the user:

- `email` **[READ-ONLY]** The user's email address
- `givenName` **[READ-ONLY]** The user's given name
- `familyName` **[READ-ONLY]** The user's family name

## Get a Profile's Users

Route:

```
GET /api/organizations/{orgID}/profiles/{profileID}/users
```

Request:

```
GET /api/organizations/21437/profiles/197715/users HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
    "email": "zoe@campdoc.com",
    "givenName": "Zoe",
    "familyName": "Washburn"
  },
  {
    "email": "hoban@campdoc.com",
    "givenName": "Hoban",
    "familyName": "Washburn"
  }
]
```

## Give a User Access to a Profile

> Note: user details are not editable once the user is created.  If `zoe@campdoc.com` already exists in the system, `givenName` and `familyName` will not be altered based on what you include in the request.  Instead, that existing user is simply granted access to the profile.

This method is recommended when setting up a single-sign-on solution (vs inviting a user, seen below).

Route:

```
POST /api/organizations/{orgID}/profiles/{profileID}/users
```

Request:

```
POST /api/organizations/21437/profiles/197715/users HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "email": "zoe@campdoc.com",
  "givenName": "Zoe",
  "familyName": "Washburn"
}
```

Response `201 Created`:

```json
{
  "email": "zoe@campdoc.com",
  "givenName": "Zoe",
  "familyName": "Washburn"
}
```

## Generate Sign-On Link (SSO)

> Generate a link to allow the specified user to sign in without entering an email address or password.  Their access for that particular session will be scoped only to the organization specified in `<orgID>`. When a user has access to multiple camps and has used an SSO link, they'll need to log out and back in again to see other camps they have access to.  SSO links are valid for 60 minutes and can only be used once.

Route:

```
POST /api/organizations/{orgID}/sso/{email}
```

Request:

```
POST /api/organizations/21437/sso/hoban@campdoc.com HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `201 Created`:

```json
{
  "url": "https://app.campdoc.com/sso/e139875-7dc9-4c9c-8a96-35df218de8cb",
  "user": {
    "email": "hoban@campdoc.com",
    "givenName": "Hoban",
    "familyName": "Washburn"
  },
  "expires": "2015-02-17T22:02:23.165Z"
}
```

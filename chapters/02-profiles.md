# Profiles

A profile object contains the following fields:

- `id` CampDoc's primary key for the profile
- `identifier` An organizational primary key for the profile
- `givenName` **[REQUIRED]**
- `middleName`
- `familyName`  **[REQUIRED]**
- `dob` Profile's date of birth, ISO-8601 formatted (e.g. "2012-01-01")
- `sex` Profile's sex, either ["Male", "Female"]
- `phase`  **[READ-ONLY]** Describes if the profile is relevant in the ["past", "present", "future"]
- `completeness` **[READ-ONLY]** The completeness of the profile's health form, from `0` - `100`

In routes that reference a specific profile, you'll find a `{profileID}` parameter, which refers to DocNetwork's `id` field on the profile.  You can replace `{profileID}` with your own `identifier`, if applicable.  The parameter *must* be prefixed with a dash (`-`) when using `identifier`.  Example:

Using DocNetwork's `id` field:

```
/api/organizations/12345/profiles/67890
```

The same profile, using your own `identifier`, "participant123abc":

```
/api/organizations/12345/profiles/-participant123abc
```

## ROUTES:

```
GET   /api/organizations/{orgID}/profiles
POST  /api/organizations/{orgID}/profiles
GET   /api/organizations/{orgID}/profiles/{profileID}
PUT   /api/organizations/{orgID}/profiles/{profileID}
GET   /api/organizations/{orgID}/profiles/{profileID}/reviews
```


## Retrieve all profiles in an organization

Route:

```
GET   /api/organizations/{orgID}/profiles
```

Request:

```
GET /api/organizations/21437/profiles/all HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
    "id": 10815,
    "profile_id": 10815,
    "givenName": "Bill",
    "middleName": null,
    "familyName": "Abbey",
    "dob": "2003-08-14",
    "sex": "Male",
    "phase": null,
    "has": {...},
    "age": 13,
    "properties": {...},
    "organizationID": 21437,
    "identifier": null,
    "completeness": 84,
    "avatar": {...},
    "profileTags": null
  },
  {
    "id": 10813,
    "profile_id": 10813,
    "givenName": "Richard",
    "middleName": null,
    "familyName": "Abbott",
    "dob": "2005-05-23",
    "sex": "Male",
    "phase": "future",
    "has": {...},
    "age": 11,
    "properties": {...},
    "organizationID": 21437,
    "identifier": null,
    "completeness": 61,
    "avatar": {...},
    "profileTags": null
  },
  ...
]
```


## Retrieve a Profile

Route:

```
GET /api/organizations/{orgID}/profiles/{profileID}
```

Request:

```
GET /organizations/21437/profiles/197715 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
{
  "id": 197715,
  "identifier": null,
  "givenName": "Malcom",
  "middleName": null,
  "familyName": "Reynolds",
  "dob": "1986-09-20",
  "sex": "Male",
  "phase": "present",
  "completeness": 100
}
```

## Create a Profile

Route:

```
POST /api/organizations/{orgID}/profiles
```

Request:

```
POST /organizations/21437/profiles HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "givenName": "Malcom",
  "familyName": "Reynolds",
  "dob": "1986-09-20",
  "sex": "Male"
}
```

Response `201 Created`:

```json
{
  "id": 197715,
  "identifier": null,
  "givenName": "Malcom",
  "middleName": null,
  "familyName": "Reynolds",
  "dob": "1986-09-20",
  "sex": "Male",
  "phase": "present"
}
```

## Update a Profile

When making updates to a record, you don't need to include the entire record or even its required fields.  A partial record will work.  If you do include a required field, it cannot be set to `null`.

```
PUT /api/organizations/{orgID}/profiles/{profileID}
```

Request:

```
PUT /organizations/21437/profiles/197715 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "middleName": "Mal"
}
```

Response `200 OK`:

```json
{
  "id": 197715,
  "identifier": null,
  "givenName": "Malcom",
  "middleName": "Mal",
  "familyName": "Reynolds",
  "dob": "1986-09-20",
  "sex": "Male",
  "phase": "present"
}
```

## Retrieve a Profile's Reviews

Route:

```
GET /api/organizations/{orgID}/profiles/{profileID}/reviews
```

Request:

```
GET /organizations/21437/profiles/197715/reviews HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
{
  "Medical": {
    "status": "Yes",
    "timestamp": "2014-07-15T13:55:33.941Z",
    "sticky": false,
    "provider": {
      "name": "Jane Smith",
      "email": "jane@campdoc.com"
    }
  },
  "General": {
    "status": "No",
    "timestamp": "2014-06-12T13:19:08.738Z",
    "sticky": true,
    "provider": {
      "name": "John Smith",
      "email": "john@campdoc.com"
    }
  }
}
```

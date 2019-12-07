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

In routes that reference a specific profile, you'll find a `{profileID}` parameter, which refers to DocNetwork's `id` field on the profile.  You can replace `{profileID}` with your own `identifier` if the target profile has been created or updated with its `identifier`. When using a profile identifier, you must include a query string parameter `?identifier=true`  Example:

Using DocNetwork's `id` field:

```
/v2/organizations/12345/profiles/67890
```

The same profile, using your own `identifier`, "participant123abc":

```
/v2/organizations/12345/profiles/participant123abc?identifier=true
```

## ROUTES:

```
GET   /v2/organizations/{orgID}/profiles
POST  /v2/organizations/{orgID}/profiles
GET   /v2/organizations/{orgID}/profiles/{profileID}
PUT   /v2/organizations/{orgID}/profiles/{profileID}
GET   /v2/organizations/{orgID}/profiles/{profileID}/reviews
```


## Retrieve all profiles in an organization

Route:

```
GET   /v2/organizations/{orgID}/profiles
```

Request:

```
GET /v2/organizations/12345/profiles HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "profiles": [
    {
      "id": 10815,
      "givenName": "Bill",
      "middleName": null,
      "familyName": "Abbey",
      "dob": "2003-08-14",
      "sex": "Male",
      "phase": null,
      "identifier": null,
      "completeness": 84,
    },
    {
      "id": 10813,
      "givenName": "Richard",
      "middleName": null,
      "familyName": "Abbott",
      "dob": "2005-05-23",
      "sex": "Male",
      "phase": "future",
      "identifier": null,
      "completeness": 61,
    },
    ...
  ]
}
```


## Retrieve a Profile

Route:

```
GET /v2/organizations/{orgID}/profiles/{profileID}
```

Request:

```
GET /organizations/12345/profiles/10813 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "profile": {
      "id": 10813,
      "givenName": "Richard",
      "middleName": null,
      "familyName": "Abbott",
      "dob": "2005-05-23",
      "sex": "Male",
      "phase": "future",
      "identifier": null,
      "completeness": 61,
    }
}
```

## Create a Profile
To create a profile, you _must_ include at least the following required fields: `givenName` and `familyName`. You may also include the other profile fields, but they aren't required.

Route:

```
POST /v2/organizations/{orgID}/profiles
```

Request:

```json
POST /organizations/12345/profiles HTTP/1.1
Host: app.campdoc.com
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
  "phase": "present",
  "completeness": 0
}
```

## Update a Profile

When making updates to a record, you only need to include the fields you want to update. That is, you _don't_ need to include the required fields or those fields you aren't updating. However, if you do include a required field, it can't be set to `null`.

```
PUT /v2/organizations/{orgID}/profiles/{profileID}
```

Request:

```json
PUT /organizations/12345/profiles/197715 HTTP/1.1
Host: app.campdoc.com
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
  "phase": "present",
  "completeness": 0
}
```

## Retrieve a Profile's Reviews

Route:

```
GET /v2/organizations/{orgID}/profiles/{profileID}/reviews
```

Request:

```
GET /organizations/12345/profiles/197715/reviews HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "profile": {
    "id": 197715,
    "identifier": null,
    "givenName": "Malcom",
    "middleName": "Mal",
    "familyName": "Reynolds",
    "dob": "1986-09-20",
    "sex": "Male",
    "phase": "present",
    "completeness": 0
  },
  "reviews": {
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
}
```

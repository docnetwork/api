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

## Retrieve a Profile

Route:

```
GET /api/organizations/<orgID>/profiles/<profileID>
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
  "phase": "present"
}
```

## Create a Profile

Route:

```
POST /api/organizations/<orgID>/profiles
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
PUT /api/organizations/<orgID>/profiles/<profileID>
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

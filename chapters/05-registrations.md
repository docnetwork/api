# Registrations

A registration object contains the following fields:

- `id` The registration's primary key
- `groupID` **[REQUIRED]** The ID of the group the profile is registered to
- `profileID` **[REQUIRED]** The ID of the profile
- `type` **[REQUIRED]** Whether the profile is registered as ["patient", "provider"]

## Review a Profile's Registrations

Route:

```
GET /api/organizations/<orgID>/profiles/<profileID>/registrations
```

Request:

```
GET /api/organizations/21437/profiles/12345/registrations HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
     "id": 1297612,
     "profileID": 12345,
     "groupID": 67890,
     "type": "patient"
  },
  {
    "id": 1297612,
    "profileID": 12345,
    "groupID": 67892,
    "type": "provider"
  }
]
```

## Register a Profile to a Group

Route:

```
POST /api/organizations/<orgID>/profiles/<profileID>/registrations
```

Request:

```
POST /api/organizations/21437/profiles/12345/registrations HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "groupID": 67890,
  "type": "patient"
}
```

Response `201 Created`:

```json
{
  "id": 1297612,
  "profileID": 12345,
  "groupID": 67890,
  "type": "patient"
}
```

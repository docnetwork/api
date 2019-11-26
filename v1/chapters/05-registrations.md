# Registrations

A registration object contains the following fields:

- `id` The registration's primary key
- `groupID` **[REQUIRED]** The ID of the group the profile is registered to
- `profileID` **[REQUIRED]** The ID of the profile
- `type` **[REQUIRED]** Whether the profile is registered as ["patient", "provider"]
- `groupName` **[READ-ONLY]** Name of the group associated with the registration
- `groupIdentifier` **[READ-ONLY]** Unique identifier of the group associated with the registration
- `created` **[READ-ONLY]** Timestamp of the registration's creation
- `updated` **[READ-ONLY]** Timestamp of when the registration was last updated
- `deactivated` Timestamp of when the registration was deactivated. `NULL` indicates an active registration

## Routes:

```
GET     /api/organizations/{orgID}/groups/{groupID}/registrations
GET     /api/organizations/{orgID}/profiles/{profileID}/registrations
POST    /api/organizations/{orgID}/profiles/{profileID}/registrations
GET     /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
PUT     /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
DELETE  /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

## Review Registrations for a Group

This route allows you to review registrations to the selected group, as well as any registrations for subgroups of the selected group.

Route:

```
GET /api/organizations/{orgID}/groups/{groupID}/registrations
```

Request:

```
GET /api/organizations/21437/groups/31258/registrations HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
    "id": 1297612,
    "profileID": 12345,
    "groupID": 31258,
    "groupName": "Session 1",
    "groupIdentifier": "sessionOneIdentifier",
    "type": "patient",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-01 17:30:17.829",
    "deactivated": null
  },
  {
    "id": 1297613,
    "profileID": 12346,
    "groupID": 31258,
    "groupName": "Session 1",
    "groupIdentifier": "sessionOneIdentifier",
    "type": "provider",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-02 07:20:17.123",
    "deactivated": null
  }
]
```

## Review a Profile's Registrations

Route:

```
GET /api/organizations/{orgID}/profiles/{profileID}/registrations
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
    "groupName": "Session 2",
    "groupIdentifier": "sessionTwoIdentifier",
    "type": "patient",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-01 17:30:17.829",
    "deactivated": null
  },
  {
    "id": 1297613,
    "profileID": 12345,
    "groupID": 67892,
    "groupName": "Session 3",
    "groupIdentifier": "sessionThreeIdentifier",
    "type": "provider",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-01 23:32:47.123",
    "deactivated": "2018-01-01 23:32:47.123"
  }
]
```

## Register a Profile to a Group

Route:

```
POST /api/organizations/{orgID}/profiles/{profileID}/registrations
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
  "groupName": "Session 2",
  "groupIdentifier": "sessionTwoIdentifier",
  "type": "patient",
  "created": "2018-01-01 17:30:17.829",
  "updated": "2018-01-01 17:30:17.829",
  "deactivated": null
}
```

> Note: You may also create registrations using your custom group identifier in place of DocNetwork's internal ID. When registering with an identifier, you must use a `groupIdentifier` key in your request body. If both a `groupID` and `groupIdentifier` are present in a request body, the `groupIdentifier` will take priority.

Request:

```
POST /api/organizations/21437/profiles/12345/registrations HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "groupIdentifier": "sessionTwoIdentifier",
  "type": "patient"
}
```

Response `201 Created`:

```json
{
  "id": 1297612,
  "profileID": 12345,
  "groupID": 67890,
  "groupName": "Session 2",
  "groupIdentifier": "sessionTwoIdentifier",
  "type": "patient",
  "created": "2018-01-01 17:30:17.829",
  "updated": "2018-01-01 17:30:17.829",
  "deactivated": null
}
```

## Review a Specific Registration

Route:

```
GET /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Request:

```
GET /api/organizations/21437/profiles/12345/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
{
  "id": 1297612,
  "profileID": 12345,
  "groupID": 67890,
  "groupName": "Session 2",
  "groupIdentifier": "sessionTwoIdentifier",
  "type": "patient",
  "created": "2018-01-01 17:30:17.829",
  "updated": "2018-01-01 17:30:17.829",
  "deactivated": null
}
```

## Modify a Registration

Route:

```
PUT /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Request:

```
PUT /api/organizations/21437/profiles/12345/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "groupID": 67891
}
```

Response `200 OK`:

```json
{
  "id": 1297612,
  "profileID": 12345,
  "groupID": 67891,
  "groupName": "Session 3",
  "groupIdentifier": "sessionThreeIdentifier",
  "type": "patient",
  "created": "2018-01-01 17:30:17.829",
  "updated": "2018-02-01 11:35:50.123",
  "deactivated": null
}
```

> Note: You may also update the group for a registration using your custom group identifier in place of DocNetwork's internal ID. When updating a registration using identifier, you must use a `groupIdentifier` key in your request body. If both a `groupID` and `groupIdentifier` are present in a request body, the `groupIdentifier` will take priority.

Request:

```
PUT /api/organizations/21437/profiles/12345/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "groupIdentifier": "sessionThreeIdentifier"
}
```

Response `200 OK`:

```json
{
  "id": 1297612,
  "profileID": 12345,
  "groupID": 67891,
  "groupName": "Session 3",
  "groupIdentifier": "sessionThreeIdentifier",
  "type": "patient",
  "created": "2018-01-01 17:30:17.829",
  "updated": "2018-02-01 11:35:50.123",
  "deactivated": null
}
```

## Deactivate a Registration

Route:

```
DELETE /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Request:

```
DELETE /api/organizations/21437/profiles/12345/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json
```

Response `204`

## Reactivate a Registration

Route:

```
PUT /api/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Request:

```
PUT /api/organizations/21437/profiles/12345/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "deactivated": null
}
```

Response `200 OK`:

```json
{
  "id": 1297612,
  "profileID": 12345,
  "groupID": 67891,
  "groupName": "Session 3",
  "groupIdentifier": "sessionThreeIdentifier",
  "type": "patient",
  "created": "2018-01-01 17:30:17.829",
  "updated": "2018-02-02 13:05:01.456",
  "deactivated": null
}
```

# Registrations

A registration object contains the following fields:

- `id` CampDoc's primary key for the registration
- `profileID` **[REQUIRED]** The ID of the profile
- `groupID` **[REQUIRED]** The ID of the group the profile is registered to
- `type` **[REQUIRED]** Whether the profile is registered as ["patient", "provider"]
- `waitlisted` **[READ-ONLY]** Boolean stating if the profile's registration is on a waitlist
- `groupName` **[READ-ONLY]** Name of the group associated with the registration
- `groupIdentifier` **[READ-ONLY]** Unique identifier of the group associated with the registration
- `created` **[READ-ONLY]** Timestamp of the registration's creation
- `updated` **[READ-ONLY]** Timestamp of when the registration was last updated
- `deactivated` Timestamp of when the registration was deactivated. `NULL` indicates an active registration

## Routes:

```
GET     /v2/organizations/{orgID}/groups/{groupID}/registrations
GET     /v2/organizations/{orgID}/profiles/{profileID}/registrations
POST    /v2/organizations/{orgID}/profiles/{profileID}/registrations
GET     /v2/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
PUT     /v2/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
DELETE  /v2/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

## Review Registrations for a Group
```
GET /v2/organizations/{orgID}/groups/{groupID}/registrations
```

Accepted Scopes:
```
all:read
all:write
registrations:read
registrations:write
```


Request:

```
GET /v2/organizations/12345/groups/31258/registrations HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "registrations": [
    {
      "id": 1297612,
      "profileID": 54321,
      "groupID": 31258,
      "type": "patient",
      "waitlisted": false,
      "groupName": "Session 1",
      "groupIdentifier": "sessionOneIdentifier",
      "created": "2018-01-01 17:30:17.829",
      "updated": "2018-01-01 17:30:17.829",
      "deactivated": null
    },
    {
      "id": 1297613,
      "profileID": 54322,
      "groupID": 31258,
      "type": "provider",
      "waitlisted": false,
      "groupName": "Session 1",
      "groupIdentifier": "sessionOneIdentifier",
      "created": "2018-01-01 17:30:17.829",
      "updated": "2018-01-02 07:20:17.123",
      "deactivated": null
    }
  ]
}
```

## Review a Profile's Registrations

```
GET /v2/organizations/{orgID}/profiles/{profileID}/registrations
```

Accepted Scopes:
```
all:read
all:write
registrations:read
registrations:write
```

Request:

```
GET /v2/organizations/12345/profiles/54321/registrations HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "registrations": [
    {
      "id": 1297612,
      "profileID": 54321,
      "groupID": 67890,
      "type": "patient",
      "waitlisted": false,
      "groupName": "Session 2",
      "groupIdentifier": "sessionTwoIdentifier",
      "created": "2018-01-01 17:30:17.829",
      "updated": "2018-01-01 17:30:17.829",
      "deactivated": null
    },
    {
      "id": 1297613,
      "profileID": 54321,
      "groupID": 67892,
      "type": "provider",
      "waitlisted": false,
      "groupName": "Session 3",
      "groupIdentifier": "sessionThreeIdentifier",
      "created": "2018-01-01 17:30:17.829",
      "updated": "2018-01-01 23:32:47.123",
      "deactivated": "2018-01-01 23:32:47.123"
    }
  ]
}
```

## Register a Profile to a Group

```
POST /v2/organizations/{orgID}/profiles/{profileID}/registrations
```

Accepted Scopes:
```
all:write
registrations:write
```

Request:

```
POST /v2/organizations/12345/profiles/54321/registrations HTTP/1.1
Host: app.campdoc.com
Content-Type: application/json

{
  "groupID": 67890,
  "type": "patient"
}
```

Response `201 Created`:

```json
{
  "registration": {
    "id": 1297612,
    "profileID": 54321,
    "groupID": 67890,
    "type": "patient",
    "waitlisted": false,
    "groupName": "Session 2",
    "groupIdentifier": "sessionTwoIdentifier",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-01 17:30:17.829",
    "deactivated": null
  }
}
```

> Custom group identifiers can be used in place of DocNetwork's internal ID. When registering with an identifier, you must use a `groupIdentifier` key in your request body. If both a `groupID` and `groupIdentifier` are present in a request body, the `groupIdentifier` will take priority.

Request:

```
POST /v2/organizations/12345/profiles/54321/registrations HTTP/1.1
Host: app.campdoc.com
Content-Type: application/json

{
  "groupIdentifier": "sessionTwoIdentifier",
  "type": "patient"
}
```

Response `201 Created`:

```json
{
  "registration": {
    "id": 1297612,
    "profileID": 54321,
    "groupID": 67890,
    "type": "patient",
    "waitlisted": false,
    "groupName": "Session 2",
    "groupIdentifier": "sessionTwoIdentifier",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-01 17:30:17.829",
    "deactivated": null
  }
}
```

## Review a Specific Registration

```
GET /v2/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Accepted Scopes:
```
all:read
all:write
registrations:read
registrations:write
```

Request:

```
GET /v2/organizations/12345/profiles/54321/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "registration": {
    "id": 1297612,
    "profileID": 54321,
    "groupID": 67890,
    "waitlisted": false,
    "groupName": "Session 2",
    "groupIdentifier": "sessionTwoIdentifier",
    "type": "patient",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-01-01 17:30:17.829",
    "deactivated": null
  }
}
```

## Deactivate a Registration

```
DELETE /v2/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Accepted Scopes:
```
all:write
registrations:write
```

Request:

```
DELETE /v2/organizations/12345/profiles/54321/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
```

Response `204 No Content`

## Reactivate a Registration

```
PUT /v2/organizations/{orgID}/profiles/{profileID}/registrations/{registrationID}
```

Accepted Scopes:
```
all:write
registrations:write
```

Request:

```
PUT /v2/organizations/12345/profiles/54321/registrations/1297612 HTTP/1.1
Host: app.campdoc.com
Content-Type: application/json

{
  "deactivated": null
}
```

Response `200 OK`:

```json
{
  "registration": {
    "id": 1297612,
    "profileID": 54321,
    "groupID": 67891,
    "waitlisted": false,
    "groupName": "Session 3",
    "groupIdentifier": "sessionThreeIdentifier",
    "type": "patient",
    "created": "2018-01-01 17:30:17.829",
    "updated": "2018-02-02 13:05:01.456",
    "deactivated": null
  }
}
```

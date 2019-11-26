# Groups

Groups represent the divisions, seasons, locations, sessions, or other sections within your organization. Group structure is represented as an unstructured tree, where the root node is a group representing the entire organization.

Example Organization Structure:
```
Discovery
└── 2017
    ├── Camper
    │   ├── Session 1
    │   └── Session 2
    └── Staff
        ├── Session 1
        └── Session 2
```

A group object contains the following fields:

- `id` The group's primary key
- `groupIdentifier` The group's unique, custom identifier
- `name` The group's name
- `parentID` The ID of the parent node
- `parents` An array of all parent node IDs (self inclusive)
- `description` User-set description of the group
- `tuition` Integer value, in cents, of the group's tuition
- `capacity` Overall capacity or sex-based capacities for the group
- `start` Date the group session begins
- `finish` Date the group session ends
- `registrationOpen` Date that participants may start registering for the group
- `registrationClose` Date that registration for the group closes
- `phase` Indicates the phase of the group. Can be `past`, `present`, or `future`
- `created` Timestamp of the group's creation
- `updated` Timestamp of the group's last update

Groups are read-only, and are returned as a flat array of nodes.

## Routes:

```
GET     /api/organizations/{orgID}/groups
GET     /api/organizations/{orgID}/groups/{groupID}
```
## Retrieve all groups in an organization
Groups returned by this route have a truncated set of properties. The properties that will be included are `id`, `name`, `parentID`, `parents`, and `groupIdentifier`. For the full set of a group's properties, use the [retrieve group](#retrieve_group) route.

Request:

```
GET /api/organizations/21437/groups HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
    "id": 21437,
    "name": "Discovery",
    "parentID": null,
    "parents": [
      21437
    ],
    "groupIdentifier": null
  },
  {
    "id": 81255,
    "name": "2017",
    "parentID": 21437,
    "parents": [
      21437,
      81255
    ],
    "groupIdentifier": null
  },
  {
    "id": 81256,
    "name": "Camper",
    "parentID": 81255,
    "parents": [
      21437,
      81255,
      81256
    ],
    "groupIdentifier": "2017CamperIdentifier"
  },
  {
    "id": 81257,
    "name": "Staff",
    "parentID": 81255,
    "parents": [
      21437,
      81255,
      81257
    ],
    "groupIdentifier": null
  },
  {
    "id": 81258,
    "name": "Session 1",
    "parentID": 81256,
    "parents": [
      21437,
      81255,
      81256,
      81258
    ],
    "groupIdentifier": "sessionOneIdentifier"
  },
  {
    "id": 81259,
    "name": "Session 2",
    "parentID": 81256,
    "parents": [
      21437,
      81255,
      81256,
      81259
    ],
    "groupIdentifier": "sessionTwoIdentifier"
  },
  {
    "id": 81266,
    "name": "Session 1",
    "parentID": 81257,
    "parents": [
      21437,
      81255,
      81257,
      81266
    ],
    "groupIdentifier": null
  },
  {
    "id": 81267,
    "name": "Session 2",
    "parentID": 81257,
    "parents": [
      21437,
      81255,
      81257,
      81267
    ],
    "groupIdentifier": null
  }
]
```

## <a name="retrieve_group"></a> Retrieve a group

Request:

```
GET /api/organizations/21437/groups/81258 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
{
  "id": 81258,
  "groupIdentifier": "sessionOneIdentifier",
  "name": "Session 1",
  "description": "Come for some fun in the sun at Camp Discovery!",
  "tuition": 1000,
  "capacity": {
    "female": "20",
    "male": ""
  },
  "parentID": 81256,
  "parents": [
    21437,
    81255,
    81256,
    81258
  ],
  "start": "2017-06-23",
  "finish": "2017-07-03",
  "registrationOpen": "2017-04-01",
  "registrationClose": "2017-06-01",
  "phase": "past",
  "created": "2016-12-14T20:08:25.345Z",
  "updated": "2017-09-01T14:34:49.796Z"
}
```

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
- `name` The group's name
- `parentID` The ID of the parent node
- `parents` An array of all parent node IDs (self inclusive)

Groups are read-only, and are returned as a flat array of nodes.

## Routes:

```
GET     /api/organizations/{orgID}/groups
```

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
    ]
  },
  {
    "id": 81255,
    "name": "2017",
    "parentID": 21437,
    "parents": [
      21437,
      81255
    ]
  },
  {
    "id": 81256,
    "name": "Camper",
    "parentID": 81255,
    "parents": [
      21437,
      81255,
      81256
    ]
  },
  {
    "id": 81257,
    "name": "Staff",
    "parentID": 81255,
    "parents": [
      21437,
      81255,
      81257
    ]
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
    ]
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
    ]
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
    ]
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
    ]
  }
]
```

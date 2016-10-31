## Retrieve an organization's Group Tags

> Note: This route will only return active tags by default. You may include one of two query parameters to modify the response object: `&includeDeactivated` and `&onlyDeactivated`

Route:
```
GET /api/organizations/{orgID}/tags
```
Request:
```
GET /api/organizations/21437/tags HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```
Response `200 OK`:
```json
[
  {
    "id": 1234,
    "groupID": 21437,
    "parentID": null,
    "value": "Lodging",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-02T14:06:13.088Z",
    "deactivated": null
  },
  {
    "id": 1235,
    "groupID": 21437,
    "parentID": 1234,
    "value": "Cabin 1",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-02T14:06:13.088Z",
    "deactivated": null
  },
  {
    "id": 1236,
    "groupID": 21437,
    "parentID": 1234,
    "value": "Cabin 2",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-02T14:06:13.088Z",
    "deactivated": null
  },
  {
    "id": 1237,
    "groupID": 21437,
    "parentID": null,
    "value": "Nickname",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-02T14:06:13.088Z",
    "deactivated": null
  }
]
```

## Create a new Group Tag

> Note: This route will accept either a JSON object or an array of JSON objects to create one or more tags. If one fails to save it will not interrupt the creation of other tags. After all request objects have been processed, the response object will return with two arrays representing what did and did not save. The `failed` array will contain error messages to help debug issues.

> If you need to know what is allowed and disallowed when creating tags, view the `Configuration` section in the [main tags documentation](/chapters/06-tags.md).

Route:
```
POST /api/organizations/{orgID}/tags
```

Request:
```
POST /api/organizations/21437/tags HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw

[
  {
    "value": "Transportation"
  },
  {
    "value": "Cabin 3",
    "parentID": 1234
  }
]
```

Response `201 Created`:
```json
{
  "failed": [],
  "saved": [
    {
      "id": 1238,
      "groupID": 21437,
      "parentID": null,
      "value": "Transportation",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 1239,
      "groupID": 21437,
      "parentID": 1234,
      "value": "Cabin 3",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    }
  ]
}
```
## Get an individual Group Tag

> Note: PUT and DELETE support is not currently available for this route. It is planned for a future release. If you urgently need this support for your use, please reach out to your API contact.

Route:
```
GET /api/organizations/{orgID}/tags/tag/{tagID}
```

Request:
```
GET /api/organizations/21437/tags/tag/1239 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:
```json
{
  "id": 1239,
  "groupID": 21437,
  "parentID": 1234,
  "value": "Cabin 3",
  "created": "2016-09-02T14:06:13.088Z",
  "updated": "2016-09-02T14:06:13.088Z",
  "deactivated": null
}
```

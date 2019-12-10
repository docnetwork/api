## Retrieve a profile's tags

> Note: This route will only return active tags by default. You may include one of two query parameters that can modify the response object: `&includeDeactivated` and `&onlyDeactivated`

Route:
```
GET /api/organizations/{orgID}/profiles/{profileID}/tags
```
Request:
```
GET /api/organizations/21437/profiles/543210/tags HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```
Response `200 OK`:
```json
[
  {
    "id": 217766,
    "profileID": 543210,
    "tagID": 3456,
    "optionID": null,
    "value": "Steve-o",
    "created": "2016-10-25T14:54:22.249Z",
    "updated": "2016-10-25T14:54:22.249Z",
    "deactivated": null
  },
  {
    "id": 202638,
    "profileID": 543210,
    "tagID": 4567,
    "optionID": 4568,
    "value": null,
    "created": "2016-07-13T20:51:03.457Z",
    "updated": "2016-07-13T20:51:03.457Z",
    "deactivated": null
  }
]
```

## Create a new Profile Tag

> Note: This route will accept either a JSON object or an array of JSON objects to create one or more tags. If one fails to save it will not interrupt the creation of other tags. After all request objects have been processed, the response object will return with two arrays representing what did and did not save. The `failed` array will contain error messages to help debug issues.

> If you need to know what is allowed and disallowed when creating tags, view the `Configuration` section in the [main tags documentation](/v1/chapters/06-tags.md).

Route:
```
POST /api/organizations/{orgID}/profiles/{profileID}/tags
```
Request:
```
POST /api/organizations/21437/profiles/543210/tags HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw

[
  {
    "tagID": 1234,
    "optionID": 1235
  },
  {
    "tagID": 2345,
    "value": "Green"
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
      "profileID": 543210,
      "tagID": 1234,
      "optionID": 1235,
      "value": null,
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 1239,
      "profileID": 543210,
      "tagID": 2345,
      "optionID": null,
      "value": "Green",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    }
  ]
}
```

## Get an individual Profile Tag

Route:
```
GET /api/organizations/{orgID}/profiles/{profileID}/tags/tag/{tagID}
```

Request:
```
GET /api/organizations/21437/profiles/543210/tags/tag/1239 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:
```json
{
  "id": 1239,
  "profileID": 543210,
  "tagID": 2345,
  "optionID": null,
  "value": "Green",
  "created": "2016-09-02T14:06:13.088Z",
  "updated": "2016-09-02T14:06:13.088Z",
  "deactivated": null
}
```

## Deactivate an individual Profile Tag

Route:
```
DELETE /api/organizations/{orgID}/profiles/{profileID}/tags/tag/{tagID}
```

Request:
```
DELETE /api/organizations/21437/profiles/543210/tags/tag/1239 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:
```json
{
  "id": 1239,
  "profileID": 543210,
  "tagID": 2345,
  "optionID": null,
  "value": "Green",
  "created": "2016-09-02T14:06:13.088Z",
  "updated": "2016-09-03T15:10:20.090Z",
  "deactivated": "2016-09-03T15:10:20.090Z"
}
```

## Update an individual Profile Tag

> Note: not all fields are required in the body to update a tag. To reactivate a deactivated tag, pass `"deactivated":null` as part of the request body.

Route:
```
PUT /api/organizations/{orgID}/profiles/{profileID}/tags/tag/{tagID}
```

Request:
```
PUT /api/organizations/21437/profiles/543210/tags/tag/1239 HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw

{
  "value": "Purple",
  "deactivated": null
}
```

Response `200 OK`:
```json
{
  "id": 1239,
  "profileID": 543210,
  "tagID": 2345,
  "optionID": null,
  "value": "Purple",
  "created": "2016-09-02T14:06:13.088Z",
  "updated": "2016-09-03T15:10:20.090Z",
  "deactivated": null
}
```

## Retrieve a profile's tags

```
GET /v2/organizations/{orgID}/profiles/{profileID}/tags
```

> This route supports a query parameter `?excludeDeactivated`, which will filter deactivated tags out of the response body.

Accepted Scopes:
```
all:read
all:write
profile_tags:read
profile_tags:write
```

Request:
```
GET /v2/organizations/12345/profiles/332211/tags HTTP/1.1
Host: app.campdoc.com
```
Response `200 OK`:
```json
{
  "profileTags": [
    {
      "id": 112233,
      "profileID": 332211,
      "tagID": 54324,
      "optionID": null,
      "value": "Steve-o",
      "created": "2016-10-25T14:54:22.249Z",
      "updated": "2016-10-25T14:54:22.249Z",
      "deactivated": null
    },
    {
      "id": 112234,
      "profileID": 332211,
      "tagID": 54321,
      "optionID": 54322,
      "value": null,
      "created": "2016-07-13T20:51:03.457Z",
      "updated": "2016-07-13T20:51:03.457Z",
      "deactivated": null
    }
  ]
}
```

## Create a new Profile Tag

```
POST /v2/organizations/{orgID}/profiles/{profileID}/tags
```

> This route will accept either a JSON object or an array of JSON objects to create one or more tags. If one fails to save it will not interrupt the creation of other tags. After all request objects have been processed, the response object will return with two arrays representing what did and did not save. The `failed` array will contain error messages to help debug issues.
>
> If you need to know what is allowed and disallowed when creating tags, view the `Configuration` section in the [main tags documentation](/v2/chapters/tags-overview.md).

Request:
```
POST /v2/organizations/12345/profiles/332211/tags HTTP/1.1
Host: app.campdoc.com

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
  "profileTags": [
    {
      "id": 1238,
      "profileID": 332211,
      "tagID": 1234,
      "optionID": 1235,
      "value": null,
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 1239,
      "profileID": 332211,
      "tagID": 2345,
      "optionID": null,
      "value": "Green",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    }
  ],
  "failed": []
}
```

## Get an individual Profile Tag

```
GET /v2/organizations/{orgID}/profiles/{profileID}/tags/{profileTagID}
```

Accepted Scopes:
```
all:read
all:write
profile_tags:read
profile_tags:write
```

Request:
```
GET /v2/organizations/12345/profiles/332211/tags/1239 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:
```json
{
  "profileTag": {
    "id": 1239,
    "profileID": 332211,
    "tagID": 2345,
    "optionID": null,
    "value": "Green",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-02T14:06:13.088Z",
    "deactivated": null
  }
}
```

## Deactivate an individual Profile Tag

```
DELETE /v2/organizations/{orgID}/profiles/{profileID}/tags/{profileTagID}
```

Accepted Scopes:
```
all:write
profile_tags:write
```

Request:
```
DELETE /v2/organizations/12345/profiles/332211/tags/1239 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:
```json
{
  "profileTag": {
    "id": 1239,
    "profileID": 332211,
    "tagID": 2345,
    "optionID": null,
    "value": "Green",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-03T15:10:20.090Z",
    "deactivated": "2016-09-03T15:10:20.090Z"
  }
}
```

## Update an individual Profile Tag

```
PUT /v2/organizations/{orgID}/profiles/{profileID}/tags/{profileTagID}
```

> Not all fields are required in the body to update a tag. To reactivate a deactivated tag, pass `"deactivated":null` as part of the request body.


Request:
```
PUT /v2/organizations/12345/profiles/332211/tags/1239 HTTP/1.1
Host: app.campdoc.com

{
  "value": "Purple",
  "deactivated": null
}
```

Response `200 OK`:
```json
{
  "profileTag": {
    "id": 1239,
    "profileID": 332211,
    "tagID": 2345,
    "optionID": null,
    "value": "Purple",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-03T15:10:20.090Z",
    "deactivated": null
  }
}
```

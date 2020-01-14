## Retrieve an organization's Group Tags

```
GET /v2/organizations/{orgID}/tags
```

> This route supports a query parameter `?excludeDeactivated`, which will filter deactivated tags out of the response body.

Accepted Scopes:
```
all:read
all:write
group_tags:read
group_tags:write
```

Request:
```
GET /v2/organizations/12345/tags HTTP/1.1
Host: app.campdoc.com
```
Response `200 OK`:
```json
{
  "groupTags": [
    {
      "id": 54321,
      "groupID": 12345,
      "parentID": null,
      "value": "Lodging",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 54322,
      "groupID": 12345,
      "parentID": 54321,
      "value": "Cabin 1",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 54323,
      "groupID": 12345,
      "parentID": 54321,
      "value": "Cabin 2",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 54324,
      "groupID": 12345,
      "parentID": null,
      "value": "Nickname",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    }
  ]
}
```

## Create a new Group Tag

```
POST /v2/organizations/{orgID}/tags
```

> This route will accept either a JSON object or an array of JSON objects to create one or more tags. If one fails to save it will not interrupt the creation of other tags. After all request objects have been processed, the response object will return with two arrays representing what did and did not save. The `failed` array will contain error messages to help debug issues.
>
> If you need to know what is allowed and disallowed when creating tags, view the `Configuration` section in the [main tags documentation](/v2/chapters/tags-overview.md).

Accepted Scopes:
```
all:write
group_tags:write
```


Request:
```
POST /v2/organizations/12345/tags HTTP/1.1
Host: app.campdoc.com

[
  {
    "value": "Transportation"
  },
  {
    "value": "Cabin 3",
    "parentID": 54321
  }
]
```

Response `201 Created`:
```json
{
  "groupTags": [
    {
      "id": 54325,
      "groupID": 12345,
      "parentID": null,
      "value": "Transportation",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    },
    {
      "id": 54326,
      "groupID": 12345,
      "parentID": 54321,
      "value": "Cabin 3",
      "created": "2016-09-02T14:06:13.088Z",
      "updated": "2016-09-02T14:06:13.088Z",
      "deactivated": null
    }
  ],
  "failed": []
}
```
## Get an individual Group Tag

```
GET /v2/organizations/{orgID}/tags/{tagID}
```

Accepted Scopes:
```
all:read
all:write
group_tags:read
group_tags:write
```

Request:
```
GET /v2/organizations/12345/tags/54326 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:
```json
{
  "groupTag": {
    "id": 54326,
    "groupID": 12345,
    "parentID": 54321,
    "value": "Cabin 3",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-02T14:06:13.088Z",
    "deactivated": null
  }
}
```

## Deactivate an individual Group Tag

```
DELETE /v2/organizations/{orgID}/tags/{tagID}
```

Accpeted Scopes:
```
all:write
group_tags:write
```

Request:
```
DELETE /v2/organizations/12345/tags/54326 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:
```json
{
  "groupTag": {
    "id": 54326,
    "groupID": 12345,
    "parentID": 54321,
    "value": "Cabin 3",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-03T15:01:11.092Z",
    "deactivated": "2016-09-03T15:01:11.092Z"
  }
}
```


## Update an individual Group Tag

```
PUT /v2/organizations/{orgID}/tags/{tagID}
```

> Not all fields are required in the body to update a tag. `ID` fields cannot be updated. To reactivate a deactivated tag, pass `"deactivated":null` as part of the request body.

Accepted Scopes:
```
all:write
group_tags:write
```


Request:
```
PUT /v2/organizations/12345/tags/54326 HTTP/1.1
Host: app.campdoc.com

{
  "value": "Cabin 16",
  "deactivated": null
}
```

Response `200 OK`:
```json
{
  "groupTag": {
    "id": 54326,
    "groupID": 12345,
    "parentID": 54321,
    "value": "Cabin 16",
    "created": "2016-09-02T14:06:13.088Z",
    "updated": "2016-09-03T15:02:13.092Z",
    "deactivated": null
  }
}
```

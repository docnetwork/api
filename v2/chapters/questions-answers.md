# Questions & Answers

A question object contains the following fields:

- `id` The question's primary key
- `label` Label or title for the question

An answer object contains the following fields:

- `profileID` ID of the profile the answer belongs to
- `questionID` ID of the question the answer is for
- `value` User-input answer value
- `created` Timestamp of when the answer was created
- `updated` Timestamp of when the answer was last updated


## Routes:

```
GET /v2/organizations/{orgID}/questions
GET /v2/organizations/{orgID}/questions/{questionID}/answers
```

## Get all questions for an organization

```
GET /v2/organizations/{orgID}/questions
```

Accepted Scopes:
```
all:read
all:write
answers:read
answers:write
```

Request:

```
GET /v2/organizations/12345/questions HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "questions": [
    {
      "id": 54321,
      "label": "Question 1"
    },
    {
      "id": 54322,
      "label": "Question 2"
    }
  ]
}
```

## Get answers for a question

```
GET /v2/organizations/{orgID}/questions/{questionID}/answers
```

> This route accepts an optional `updated` query string to filter returned answers to only those updated since the provided date. The format of the query string must be `YYYY-MM-DD`.

Accepted Scopes:
```
all:read
all:write
answers:read
answers:write
```

Request:

```
GET /v2/organizations/12345/questions/54321/answers HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "answers": [
    {
      "questionID": 54321,
      "profileID": 6789,
      "value": "Answer 1",
      "created": "2017-12-01T13:23:57.57",
      "updated": "2017-12-15T21:14:00.01"
    },
    {
      "questionID": 54321,
      "profileID": 1012,
      "value": "Answer 2",
      "created": "2017-12-01T13:23:57.57",
      "updated": "2017-12-01T13:23:57.57"
    },
    {
      "questionID": 54321,
      "profileID": 3456,
      "value": "Answer 3",
      "created": "2015-06-27T00:09:48.13",
      "updated": "2017-12-01T00:09:48.13"
    }
  ]
}
```

Request:

```
GET /v2/organizations/12345/questions/54321/answers?updated=2017-12-02 HTTP/1.1
Host: app.campdoc.com
```

Response `200 OK`:

```json
{
  "answers": [
    {
      "questionID": 54321,
      "profileID": 6789,
      "value": "Answer 1",
      "created": "2017-12-01T13:23:57.57",
      "updated": "2017-12-15T21:14:00.01"
    }
  ]
}
```

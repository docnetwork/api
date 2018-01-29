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
GET /api/organizations/{orgID}/questions
GET /api/organizations/{orgID}/questions/{questionID}/answers
```

## Get all questions for an organization

Route:

```
GET /api/organizations/{orgID}/questions
```

Request:

```
GET /api/organizations/21437/questions HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
    "id": 12345,
    "label": "Question 1"
  },
  {
    "id": 12346,
    "label": "Question 2"
  }
]
```

## Get all answers for a question

Route:

```
GET /api/organizations/{orgID}/questions/{questionID}/answers
```

Request:

```
GET /api/organizations/21437/questions/12345/answers HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Response `200 OK`:

```json
[
  {
    "questionID": 12345,
    "profileID": 6789,
    "value": "Answer 1",
    "created": "2017-12-01 13:23:57.576",
    "updated": "2017-12-15 21:14:00.012"
  },
  {
    "questionID": 12345,
    "profileID": 1012,
    "value": "Answer 2",
    "created": "2017-12-01 13:23:57.576",
    "updated": "2017-12-01 13:23:57.576"
  },
  {
    "questionID": 12345,
    "profileID": 3456,
    "value": "Answer 3",
    "created": "2015-06-27 00:09:48.132",
    "updated": "2017-12-01 00:09:48.132"
  }
]
```

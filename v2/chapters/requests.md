# Requests

The DocNetwork API is organized around [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Our API uses standard HTTP response codes, authentication, and verbs, and accepts and returns data in a [JSON](https://en.wikipedia.org/wiki/JSON) format. Here, we'll describe the components of every request you'll send.

## Authorization

In order to use the API, you will need to set up an OAuth2 application. Please see the [Authorization](./oauth.md) page of this documentation for more information. Assume all samples in this documentation include a header `Authorization: bearer <access_token>`.

## The Base URL

The application and its API is currently available at two different domains:

- https://app.campdoc.com
- https://app.schooldoc.com

These domains can be used interchangeably.  You'll be accessing the same application with the same data regardless of the domain you choose to use.  Schools may feel more inclined to access the API through the SchoolDoc domain, and camps through CampDoc.

The inclusion of one of these domains is assumed throughout the rest of the documentation.  This API route:

```
GET https://app.campdoc.com/public/v2/organizations/{orgID}/profiles/{profileID}
```

will only be referred to by its pathname:

```
GET /v2/organizations/{orgID}/profiles/{profileID}
```

## Parameters

All routes make use of parameters. Parameters are marked with curly brackets (`{` and `}`) where routes are defined in the documentation.  For instance, the following route contains two parameters, `orgID` and `profileID`:

```
GET /v2/organizations/{orgID}/profiles/{profileID}/users
```

When calling the route, include an appropriate `orgID` and `profileID`:

```
GET /v2/organizations/12345/profiles/54321/users
```

## Request Methods

We use four request methods in our API: `GET`, `POST`, `PUT`, and `DELETE`.  For this API, these generally mean:

- `GET` &rarr; Retreive a resource or resources
- `POST` &rarr; Create a new resource or resources
- `PUT` &rarr; Modify a resource
- `DELETE` &rarr; Deactivate a resource

## Permissions Structure

DocNetwork has a robust model for data access permissions.  There are three types of objects you'll need to get acquainted with to make full use of the system: users, profiles, and registrations.

### Users

Users include everyone that interacts with the system, from parents and providers to machine users that might be created solely for the purpose of API access.  Information associated with a user can include name and email address.

### Profiles

Profiles are the "people" that populate an organization, and includes campers, staff, and medical providers. Users may be associated with multiple profiles, and a profile may have multiple users.

### Registrations

Profiles can have any number of registrations to different groups within the system. There are two `type`s of registrations: `patient` and `provider`.  Examples include:

- **A camper, Timmy**, is registered as a patient for "Session 1" and "Session 2" of Discovery Camps. Timmy's dad has a user account under his own name and is able to interact with Timmy's health form. Providers are able to add health log entries and eMAR records to Timmy's account.

- **A nurse, Sally**, is registered as a provider to Discovery Camps as a whole.  She is able to see every patient in the organization and interact with their medical records.  Nurse Sally has her own user record (in addition to her profile) that she uses to log in to access the system.

- **Another nurse, John**, will only be working part of the summer.  He has two provider registrations, under "Session 2" and "Session 3".  John is only able to see patients registered under those two sessions.  John is Timmy's dad, and his user is associated with both the "John" profile (allowing provider access) and the "Timmy" profile (allowing patient/parent access).  He is able to interact with only "Session 2" and "Session 3" campers, and only while those sessions are taking place.

## Supported Headers
### Distributed Tracing
Our API supports the [W3C `traceparent` header specification.](https://www.w3.org/TR/trace-context/#traceparent-header)
#### What is this for?
When you send us an API request, you _can_ (but don't need to) attach a unique identifier to it.
#### How do I use it?

#### Examples
- Request with empty header
- Request with improperly formatted header
  - 400 Response
  - Specific message? Or just "Bad Request"?
- Request with properly formatted header
- Response with properly formatted header
### Content-Type

When sending a `POST` or `PUT` request, data should be in a JSON format in your request body.  When making these requests, be sure to add an appropriate `Content-Type` header specifying `application/json`.  You'll see an example of this in the `POST` request in the examples section below.

### Rate Limiting

Each application is limited to 2500 requests every 15 minutes, which is slightly more than 2.5 requests per second. This applies to all methods, and exceeding the number of allotted requests will result in a `429` response code for any request sent until the request volume falls below the limit.

We include custom headers to facilitate rate limiting:
- `X-RateLimit-Limit` contains the number of allotted requests for a 15 minute interval
- `X-RateLimit-Remaining` contains the number of remaining requests for the current 15 minute interval
- If the allotted number of requests is exceeded, the `Retry-After` header will indicate when more requests will be available

## Example Requests

The following are examples of valid http requests through our API.

Retrieving information on users associated with profile `54321`, who belongs to organization `12345`:

```
GET /v2/organizations/12345/profiles/54321/users HTTP/1.1
Host: app.campdoc.com
```

Responds with
```json
{
  "users": [
    {
      "email": "test@example.com",
      "givenName": "Test",
      "familyName": "Example"
    },
    {
      "email": "example@test.com",
      "givenName": "Example",
      "familyName": "Test"
    }
  ]
}
```

Creating a new profile under organization `12345`:

```
POST /v2/organizations/12345/profiles HTTP/1.1
Host: app.campdoc.com
Content-Type: application/json
```

```json
{
  "givenName": "Malcom",
  "familyName": "Reynolds",
  "dob": "1986-09-20"
}
```
Responds with
```json
{
  "profile": {
    "id": 54322,
    "phase": "present",
    "completeness": null,
    "identifier": null,
    "givenName": "Malcom",
    "middleName": null,
    "familyName": "Reynolds",
    "dob": "1986-09-20",
    "sex": null
  }
}
```

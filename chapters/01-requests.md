# Requests

Calls to our API are made with REST style http requests.  Here, we'll describe the components of every request you'll send.

## The Base URL

The application and its API is currently available at two different domains:

- https://app.campdoc.com
- https://app.schooldoc.com

These domains can be used interchangeably.  You'll be accessing the same application with the same data regardless of the domain you choose to use.  Schools may feel more inclined to access the API through the SchoolDoc domain, and camps through CampDoc.

The inclusion of one of these domains is assumed throughout the rest of the documentation.  This API route:

```
GET https://app.campdoc.com/api/organizations/{orgID}/profiles/{profileID}
```

would only be referred to by its pathname:

```
GET /api/organizations/{orgID}/profiles/{profileID}
```

## Parameters

All routes make use of parameters. Parameters are marked with angle brackets (`{` and `}`) where routes are defined in the documentation.  For instance, the following route contains two parameters, `orgID` and `profileID`:

```
GET /api/organizations/{orgID}/profiles/{profileID}/users
```

When we actually call that route, we'll include an appropriate `orgID` and `profileID`, which will properly point to the resource(s) we want:

```
GET /api/organizations/21437/profiles/12345/users
```

## Request Methods

We use four request methods in our API: `GET`, `POST`, `PUT`, and `DELETE`.  For us, these generally mean:

- `GET` &rarr; Retreives a resource and has no other effect
- `POST` &rarr; Create a new resource
- `PUT` &rarr; Modify a resource
- `DELETE` &rarr; Delete (usually just deactivate) a resource

## Permissions Structure

DocNetwork has a robust model for data access permissions.  There are three types of objects you'll need to get acquainted with to make full use of the system: users, profiles, and registrations.

### Users

Users include everyone that interacts with the system, from parents and providers to machine users that might be created solely for the purpose of API access.  Information associated with a user can include name, email address, password, and an API key.

### Profiles

Profiles are the "people" that populate an organization, and includes campers, staff, and medical providers. Users can be associated with multiple profiles.

### Registrations

Profiles can have any number of registrations to different groups within the system. Registrations come in two flavors: `patient` and `provider`.  Examples include:

- **A camper, Timmy**, is registered as a patient for "Session 1" and "Session 2" of Discovery Camps.  Timmy's parents are able to fill out a health form, and providers are able to add health log entries and eMAR records under his name.  Timmy's dad has a user account under his own name and is able to interact with Timmy's health form.

- **A nurse, Sally**, is registered as a provider to Discovery Camps as a whole.  She is able to see every patient in the organization and interact with their medical records.  Nurse Sally has her own user record (in addition to her profile) that she uses to log in to access the system.  She is able to interact with the whole camp.

- **Another nurse, John**, will only be working part of the summer.  He has two provider registrations, under "Session 2" and "Session 3".  John is only able to see patients registered under those two sessions.  John is Timmy's dad, and his user is associated with both the "John" profile (allowing provider access) and the "Timmy" profile (allowing patient/parent access).  He is able to interact with only "Session 2" and "Session 3" campers, and only while those sessions are taking place.

## Authorization

In order to use the API, you'll need the credentials of a user who has access to a profile that is registered at some level of the organization as a `provider`.

The two important pieces of information you'll need from the user to make authorized requests are the user's `id` and `apiKey`.  You'll use these two pieces of information to create a Base64-encoded authorization string.  For example:

- User ID: `12345`
- API Key: `5596af0a-1356-46d1-b217-2daa35c934f0`
- Authorization String: `12345:5596af0a-1356-46d1-b217-2daa35c934f0`
- Authorization String (Base64): `MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw`

This authorization string must be included with every request, either as a header or query parameter:

### Authorization Header

```
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

### Authorization Query Parameter

```
?authorization=MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

## Content-Type

When sending a `POST` or `PUT` request, we're normally transmitting data.  This data should be JSON format in the body of the request.  When making these requests, be sure to add an appropriate `Content-Type` header specifying `application/json`.  You'll see an example of this in the `POST` request in the examples section, below.

## Rate Limiting

Each API key is limited to 2500 requests every 15 minutes, which is slightly more than 2.5 requests per second. This applies to all methods, and exceeding the number of allotted requests will result in a `429` response code for any request sent until the request volume falls below the limit.

## Example Requests

The following are examples of valid http requests through our API.

Retrieving information on users associated with profile `12345`, who belongs to organization `21437`:

```
GET /api/organizations/21437/profiles/12345/users HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
```

Creating a new profile under organization `21437`:

```
POST /api/organizations/21437/profiles HTTP/1.1
Host: app.campdoc.com
Authorization: Basic MTIzNDU6NTU5NmFmMGEtMTM1Ni00NmQxLWIyMTctMmRhYTM1YzkzNGYw
Content-Type: application/json

{
  "givenName": "Malcom",
  "familyName": "Reynolds",
  "dob": "1986-09-20"
}
```

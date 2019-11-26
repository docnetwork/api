# OAuth
All public API requests require authentication. Authentication is handled using the [OAuth Authorization Code](https://tools.ietf.org/html/rfc6749#section-4.1) flow with optional [PKCE](https://tools.ietf.org/html/rfc7636). Public API requests must include an access token in the request header. To use the DN public API, your app will:

1. Request an authorization code
2. Use the authorization code to request an access token
3. Use the access token to query our public API
4. Use a refresh token to automate requesting a new access token when a token expires

## Request an Authorization Code
Your app will send your user to the CampDoc/SchoolDoc website to log in with their user credentials. Note that you must log in with the user associated with your app. The user will then be able to give the app access to DN data or cancel authorization. Upon clicking a button, the user will be redirected back to your app.

The route for the Authorization Code route is `GET /!/oauth`. All parameters must be URI encoded.

| Query Parameter | Value |
| --------------- | ----- |
| client_id       | Required - The client_id provided by DN during onboarding |
| response_type   | Required - Must be `code` |
| redirect_uri    | Optional - The full URI to redirect to after the user grants or denies access to your app. This must match the URI you provided to DN during onboarding. If not provided, it will default to the value provided to DN during onboarding.
| state           | Optional - A value that will be sent back to your app on redirect. Can be used to prevent CSRF attacks. |
| scope           | Required - A space delimited list of [scopes](scopes.md) detailing the permissions your app needs |

Example:
```
https://app.campdoc.com/!/oauth?client_id=jfjf83n3nskdhsdf&response_type=code&redirect_uri=https%3A%2F%2Fexample.com%2Fcallback&scope=groups%3Aread%20profiles%3Awrite
```

If the user grants permission, they will be redirected to your redirect_uri with the following query parameters.

| Query Parameter | Value |
| --------------- | ----- |
| code            | The authorization code to use when getting a token |
| state           | The `state` value sent when requesting the code. Only sent if `state` was provided. |

Example:
```
https://example.com/callback&code=99d32d98-1848-4bee-913c-285a2f389ea3&state=STATE
```

If the user denies permission, they will be redirected to your redirect_uri with the following query parameters:

| Query Parameter   | Value |
| ----------------- | ----- |
| error             | The reason the request failed |
| error_description | Optional - Some errors will have more detailed information sent in this parameter
| state             | The `state` value sent when requesting the code. Only sent if `state` was provided. |

Example:
```
https://example.com/error=invalid_request&error_description=Invalid%20parameter%3A%20badParam&state=STATE
```

## Request an Access Token and Refresh Token
After receiving the authorization code, your app should request an access token and refresh token. The token route is `POST /public/v2/oauth/access_token`.

App credentials must be sent in the request header.

| Header Parameter | Value |
| ---------------- | ----- |
| Authorization    | Must be in the format `Basic <base64 encoded client_id:client_secret>`. |

The other parameters must be sent in the request body using the `x-www-form-urlencoded` encoding.

| Body Parameter | Value |
| -------------- | ----- |
| grant_type     | Required - Must be `authorization_code` |
| code           | Required - The code returned from the `/!/oauth` route
| redirect_uri   | Optional - The full URI that was provided when requesting an Authorization Code. If not provided, it will default to the value provided to DN during onboarding. |

Example:
```
curl -X POST \
  https://app.campdoc.com/public/v2/oauth/access_token \
  -H 'Authorization: Basic M...0' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Host: app.campdoc.com' \
  -d 'grant_type=authorization_code&code=fad5d...2020d9d&redirect_uri=https%3A%2F%2Fexample.com%2Fcallback'
```

If the request is successful, you will receive a JSON response with the tokens and other info:

| JSON Property | Description |
| ------------- | ----------- |
| access_token  | The token to use when making public API requests |
| refresh_token | The token to use when requesting a new access_token (more details to follow) |
| token_type    | Always `bearer` |
| expires_in    | The access token lifetime in seconds |
| scope         | An array of granted scopes |

Example:
```
{
    "access_token": "zBBf...7u",
    "refresh_token": "V46H...sM",
    "token_type": "bearer",
    "expires_in": 604800,
    "scope": [
        "groups:read",
        "profiles:write"
    ]
}
```

If there is an error, you will receive a JSON response with a 4xx HTTP status:

| JSON Property | Description |
| ------------- | ----------- |
| error         | The reason the request failed |
| error_description | Optional - Some errors will have more detailed information sent in this property |

Example:
```
{
    "error": "invalid_grant",
    "error_description": "Invalid credentials submitted"
}
```

## Query the Public API
Use the access token when querying any of the public API routes. The access token is given in the request header:

| Header Parameter | Value |
| ---------------- | ----- |
| Authorization    | `bearer <access_token>` |

For example, you would query the groups for org 123 like so:

```
curl -X GET \
  https://app.campdoc.com/public/v2/organizations/123/groups \
  -H 'Authorization: bearer zBBf...7u' \
  -H 'Host: app.campdoc.com'
```

## Using Refresh Tokens
A refresh token can be used to get a new access token after the original token has expired. The advantage of using a refresh token is that you won't have to manually log into CampDoc/SchoolDoc and request a new authorization code. The refresh token route is also `POST /public/v2/oauth/access_token`.

App credentials must be sent in the request header.

| Header Parameter | Value |
| ---------------- | ----- |
| Authorization    | Must be in the format `Basic <base64 encoded client_id:client_secret>`. |

The other parameters must be sent in the request body using the `x-www-form-urlencoded` encoding.

| Body Parameter | Value |
| -------------- | ----- |
| grant_type     | Required - Must be `refresh_token` |
| refresh_token  | Required - The refresh token returned with the original access token
| redirect_uri   | Optional - The full URI that was provided when requesting an Authorization Code. If not provided, it will default to the value provided to DN during onboarding. |

Example:
```
curl -X POST \
  https://app.campdoc.com/public/v2/oauth/access_token \
  -H 'Authorization: Basic M...0' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -H 'Host: app.campdoc.com' \
  -d 'grant_type=refresh_token&refresh_token=V46H...sM&redirect_uri=https%3A%2F%2Fexample.com%2Fcallback'
```

The success and error responses are in the same format as when using the authorization code to get the original access token.

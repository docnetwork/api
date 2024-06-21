# Onboarding

All active DocNetwork clients are eligible to create their own API integrations. Once your account is created in the DocNetwork app, ask your Client Success Manager about using the API. They will contact the Development Team to continue the process.

We will ask you for a single datapoint: one or more `redirect_uris`. The `redirect_uri` is where the browser will be redirected once the OAuth application is authorized on your account. This is only required for the initial setup of your account, and outside of a single redirect during setup, no requests will be sent to the URIs. These are in place to enable custom workflows if needed for your use case. Without a URI value, your integration will be created with a dummy value. The authorization process and integration setup can be completed using the dummy value, just without any custom workflows that may be present at your own endpoint.

> Please note that DocNetwork will only accept `https` values for `redirect_uris`

A Dev Team member will then provision your account for API access. This may take up to two weeks to complete. Once everything is configured, we will contact you with the application information and user login credentials needed to finalize the process. See our [Authorization](oauth.md) docs for more information.

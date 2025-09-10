## Basic Access Authentication


In the context of an HTTP transaction, **Basic Access Authentication** is a method for an HTTP user agent (e.g., a web browser) to provide a username and password when making a request.


In basic HTTP authentication, a request contains a header field in the following form:

```
Authorization: Basic <credentials>
```


where `<credentials>` is the Base64 encoding of the username and password joined by a single colon (`:`).


HTTP Basic Authentication (BA) is the simplest technique for enforcing access controls to web resources because it does not require cookies, session identifiers, or login pages. Instead, HTTP Basic Authentication uses standard fields in the HTTP header.

## OAuth 2.0



OAuth 2.0 is an authorisation framework designed to allow third-party applications to access resources on behalf of a user, without sharing the user's credentials. It introduces concepts such as:

- **Authorisation Server**: Issues tokens after authenticating the user.
- **Resource Server**: Hosts the protected resources.
- **Access Token** (often a JWT): Used by the client to access resources.
- **Authorisation Grant**: The method by which the client obtains the token (e.g., authorisation code, client credentials, etc.).


**Typical workflow:**
1. The user initiates login on the client app (e.g., clicks "Sign in with Google").
2. The client app redirects the user to the OAuth 2.0 Authorisation Server (e.g., Google) with a request for authorisation, specifying the desired scopes (the permissions or access levels the app is requesting).
3. The user authenticates (logs in).
4. The Authorisation Server displays a consent screen to the user. This screen lists what data or actions the client app is requesting (e.g., access to email, profile, calendar), based on the requested scopes.
5. The user can approve (grant) or deny these requested permissions.
6. If the user grants permission, the Authorisation Server redirects the user back to the client app’s redirect URI, including an authorisation code in the URL.
7. The client app sends this authorisation code (along with its own credentials) to the Authorisation Server’s token endpoint.
8. If the code and credentials are valid, the Authorisation Server returns an access token (often a JWT) and optionally a refresh token to the client app. The access token contains information about the user and the granted scopes.
9. The client app uses the access token to access the user’s data or perform actions on their behalf, but only within the scope of permissions the user granted. The Resource Server checks the token (and its scopes) to determine what actions are allowed.
10. If the access token expires, the client app can use the refresh token to obtain a new access token without user involvement.

**Note:**
- **JWT (JSON Web Token):** The access token is often a JWT, which securely encodes information about the user and the granted scopes. JWTs are signed to prevent tampering and can be validated by the Resource Server without contacting the Authorisation Server.
- **Scope:** The scope defines the specific permissions or resources the client app is allowed to access. The access token is only valid for the scopes granted by the user during the consent step.
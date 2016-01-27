# Authenticators and Authentication Results

This document describes the authenticators that must exist in each of our SDKs,
and the types of authentication results that the produce.  Both types of objects
are enumerated on this page, in their respective section.

Jump to:

  * [Authenticators](#authenticators)
  * [Authentication Results](#authentication-results)

The usage of an Authenticator and Authentication Result will typically look like
this:

```
AuthenticatorX authenticator = new AuthenticatorX(application)

AuthenticationResult result = authenticator.authenticate(paramA, paramN)

Account account = result.getAcount()
```


# Authenticators

An authenticator:

* Is constructed with an Application context.

* Has an `authenticate` method for performing an authentication attempt.  The
  parameters will depend on the type of authentication that is occurring.

* Responds with the appropriate authentication result.


## BasicAuthenticator

This authenticator will authenticate the API Key and Secret of a Stormpath
Account object.  Authentication should succeed only if the following are true:

* The provided API Key and Secret exist for an account that is reachable by
  the application.
* The API Key is not disabled.
* The Account is not disabled.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Authenticates the given credentials | Account API Key and Secret | AuthenticationResult |


## OAuthRequestAuthenticator

This class should authenticate OAuth2 requests.  It will *eventually* support
authenticating all 4 OAuth2 grant types.

Specifically, right now, this class will authenticate OAuth2 access tokens, as
well as handle API key for access token exchanges using the OAuth2 client
credentials grant type.

**NOTE**: In the future we might need to accept options regarding which grant
types this authenticator should support.


#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | A method used to authenticate an HTTP Request object | OAuth Authentication Request | OAuth Authentication Result |


## OAuthBearerAuthenticator

This class should authenticate OAuth2 bearer tokens only.  The token is an
access token JWT that has been created by Stormpath.  The token may have been
created by the `client_credential` or `password_grant` flow.  This can be
determined by looking at the `kid` property in the header of the JWT.  Password
grant JWTs will have a `kid`, but client credential JWTs will not.

This authenticator should only authenticate if the following is true:

* The JWT was signed by the tenant of the application
* The JWT is not expired


#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Verifies the access token JWT | Access Token JWT | AuthenticationResult |


## OAuthClientCredentialsAuthenticator

This authenticator accepts an Account's API Key and Secret, and gives back an
access token in response.  The authenticator should follow the same
authentication rules as the BasicAuthenticator.  The end-user (account) can
request scope, if the scope factory determines that this scope is permitted,
then the scope should be added to the access token.

This authenticator is responsible for creating the access token.  The Stormpath
REST API does not yet provide the `client_credential` grant on the
appplication's `/oauth/token` endpoint.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Verify client credentials | Account API Key and Secret, requested scope | OauthClientCredentialsAuthenticationResult |
| setScopeFactory | Setter method | A function which determines if the requested scope is permitted | null |
| setTtl | Setter method | The expiration time of the created access token, in seconds | null |


## OAuthPasswordAuthenticator

This authenticator accepts an account's username and password, and returns an
access token response that is obtained by posting the username and password to
the application's `/oauth/token` endpoint with the `grant_type=password`
parameter.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Posts the credentials to `:appid/oauth/token` | Account username and password | OAuthAccessTokenResult |


## OAuthRefreshTokenAuthenticator

This authenticator accepts a previously-issued refresh token and post's it to
the application's `/oauth/token` endpoint with the `grant_type=refresh_token`
parameter.  The response is a new access token response.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Posts the refresh token to `:appid/oauth/token` | Refresh Token JWT | OAuthAccessTokenResult |


## OAuthStormpathTokenAuthenticator

This authenticator takes a Stormpath Token JWT and posts it to the application's
`/oauth/token` endpoint, as `grant_type=stormpath_token`.  The result is an
OAuthAccessTokenResult.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Posts the JWT to `:appid/oauth/token` | Stormpath Token JWT | OAuthAccessTokenResult |



## StormpathAssertionAuthenticator

This class will verify the a JWT from an ID Site or SAML callback.  It should
verify that:

* The token is not expired
* The signature can be verified
* The claims body does not contain an `err` property.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | Verifies the Stormpath Token JWT | Stormpath Token JWT | StormpathAssertionAuthenticationResult |



# Authentication Results

An authentication result:

* Has a `getAccount` method

* Provides other context-specific getters

## AuthenticationResult

This generic authentication result should provide a `getAccount()` method that
allows you to get the account that has authenticated.


## OAuthAccessTokenResult

This extends the generic AuthenticationResult, and is a wrapper for the JSON
response that you get from the  application's `/oauth/token` endpoint.  When
serialized, it should return that same format:

```
{
  "refresh_token": "eyJraWQiOiI2NldURFJVM1paSkNZVFJVVlZTUUw3WEJOIiwiYWxnIjoiSFMyNTYifQ.eyJqdGkiOiIyVUdFYnBWVDlpbDYzc3Z1eGI1UjBuIiwiaWF0IjoxNDUzODUzODM1LCJpc3MiOiJodHRwczovL2FwaS5zdG9ybXBhdGguY29tL3YxL2FwcGxpY2F0aW9ucy8xaDcyUEZXb0d4SEtoeXNLallJa2lyIiwic3ViIjoiaHR0cHM6Ly9hcGkuc3Rvcm1wYXRoLmNvbS92MS9hY2NvdW50cy80V0NDdGMwb0NSRHpRZEFIVlFUcWp6IiwiZXhwIjoxNDU2NDQ1ODM1fQ.DuLqHxJUkbEAzOjFCxlEY20qMpbxcDj_7eG3Yr-dMqA",
  "stormpath_access_token_href": "https://api.stormpath.com/v1/accessTokens/2UGEbspY4J44gi1m9qwhcr",
  "token_type": "Bearer",
  "access_token": "eyJraWQiOiI2NldURFJVM1paSkNZVFJVVlZTUUw3WEJOIiwiYWxnIjoiSFMyNTYifQ.eyJqdGkiOiIyVUdFYnNwWTRKNDRnaTFtOXF3aGNyIiwiaWF0IjoxNDUzODUzODM1LCJpc3MiOiJodHRwczovL2FwaS5zdG9ybXBhdGguY29tL3YxL2FwcGxpY2F0aW9ucy8xaDcyUEZXb0d4SEtoeXNLallJa2lyIiwic3ViIjoiaHR0cHM6Ly9hcGkuc3Rvcm1wYXRoLmNvbS92MS9hY2NvdW50cy80V0NDdGMwb0NSRHpRZEFIVlFUcWp6IiwiZXhwIjoxNDUzODUzODk1LCJydGkiOiIyVUdFYnBWVDlpbDYzc3Z1eGI1UjBuIn0.o5iRrL2FYvA9XyucLG1K3wLuxSDKVU5YVjTHZR-TOWo",
  "expires_in": 60
}
```

## OauthClientCredentialsAuthenticationResult

This extends the generic AuthenticationResult, with the following methods:

This object should be serialze-able into an Access Token Response, which
looks like this:

```
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"example",
  "expires_in":3600
}
```

It should provide the following methods:

* `getAccessToken()`, which returns the `access_token` string.
* `getExpiresIn()`, which returns the `expires_in` value.
* `getTokenType()`, which returns the `token_type` value.

## StormpathAssertionAuthenticationResult

This extends the generic AuthenticationResult, and provides access to
information that is embedded in the JWT.

It should provide the following methods:

* `getState()` - returns the `state` claim of the JWT.
* `getStatus()` - returns the `status` claim of the JWT.
* `getIsNewSub()` - returns the `isNewSub` claim of the JWT.


It should provide the following methods:

* `getAccessToken()`, which returns the `access_token` string.
* `getStormpathAccessToken()`, which fetches the resource at
  `stormpath_access_token_href`.
* `getRefreshToken()`, which returns the `refresh_token` string.
* `getExpiresIn()`, which returns the `expires_in` value.
* `getTokenType()`, which returns the `token_type` value.

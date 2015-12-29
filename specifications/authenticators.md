# Authenticators

This spec references how our Stormpath libraries handles request authentication
and assertion authentication.

The various authenticator classes that each SDK should provide are enumerated
here.  These classes should be initialized with a Stormpath Application and
provide an `authenticate` method and return an `authenticationResult`.  The
`authenticationResult` must always have a `getAccount` method.  The result
may have more methods.

## BasicRequestAuthenticator

This class should authenticate HTTP Basic Auth requests, where the username and
password are the API keys of a Stormpath account.


#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | A method used to authenticate an HTTP Request object | Basic Authentication Request| Basic Authentication Result |


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


## OAuthBearerRequestAuthenticator

This class should authenticate OAuth2 bearer token requests only.  It will only
look for the bearer token in the HTTP request.  The token may have been created
by the `client_credential` or `password_grant` flow.


#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | A method used to authenticate an HTTP Request object | OAuth Bearer Authentication Request| OAuth Bearer Authentication Result |


## OAuthClientCredentialsRequestAuthenticator

This class should authenticate OAuth2 client credentials grant type requests
only.  It will handle authenticating a request based on API key credentials.


#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | A method used to authenticate an HTTP Request object | OAuth Client Credentials Authentication Request| OAuth Client Credentials Authentication Result |

## StormpathAssertionAuthenticator

This class will verify the integrity of JWTs that result from an ID Site or SAML
authentication.  The authentication result should have a `getAccessTokenResponse`
which should the response from the application's
`/oauth/token` endpoint, but making a `grant_type=stormpath&token=<JWT>`
request.

#### Methods

| Name | Description | Input | Output |
| ---- | ----------- | ----- | ------ |
| Authenticate | A method used to verify the JWT | Stormpath Assertion JWT | JWT Authentication Result |



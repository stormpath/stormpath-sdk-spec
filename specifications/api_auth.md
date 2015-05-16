# API Auth

This spec references how our Stormpath libraries handle API authentication.

Stormpath provides ways to authenticate API requests using both HTTP Basic
Authentication and OAuth2.


## Authenticators

This section discusses the various authenticator classes that each SDK should
provide.  These classes should be initialized with a Stormpath Application and
accept an HTTP request object of some sort.


### ApiRequestAuthenticator

This class should authenticate both HTTP Basic Auth *and* OAuth2 requests.
However, if you need more specific or customized OAuth2 request processing, you
will likely want to use the OauthRequestAuthenticator class.


#### Methods

TODO


### BasicRequestAuthenticator

This class should authenticate HTTP Basic Auth requests.


#### Methods

TODO


### OAuthRequestAuthenticator

This class should authenticate OAuth2 requests.  It will *eventually* support
authenticating all 4 OAuth2 grant types.

Specifically, right now, this class will authenticate OAuth2 access tokens, as
well as handle API key for access token exchanges using the OAuth2 client
credentials grant type.

**NOTE**: In the future we might need to accept options regarding which grant
types this authenticator should support.


#### Methods


### OAuthBearerRequestAuthenticator

This class should authenticate OAuth2 bearer token requests only.  It will only
look for the bearer token in the HTTP request.


#### Methods


### OAuthClientCredentialsRequestAuthenticator

This class should authenticate OAuth2 client credentials grant type requests
only.  It will handle authenticating a request based on API key credentials.

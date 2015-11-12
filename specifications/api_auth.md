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

This section lists the methods that this class should have.

| Name | Description | Input | Output | REST | Java | Python | Node | PHP | Ruby |
| ---- | ----------- | ----- | ------ | ---- | ---- | ------ | ---- | --- | ---- |
| Authenticate | A method used to authenticate an HTTP Request object | HTTP Request Object| API Authentication Result |  |  |  |  |  |  |


### BasicRequestAuthenticator

This class should authenticate HTTP Basic Auth requests.


#### Methods

This section lists the methods that this class should have.

| Name | Description | Input | Output | REST | Java | Python | Node | PHP | Ruby |
| ---- | ----------- | ----- | ------ | ---- | ---- | ------ | ---- | --- | ---- |
| Authenticate | A method used to authenticate an HTTP Request object | Basic Authentication Request| Basic Authentication Result |  |  |  |  |  |  |


### OAuthRequestAuthenticator

This class should authenticate OAuth2 requests.  It will *eventually* support
authenticating all 4 OAuth2 grant types.

Specifically, right now, this class will authenticate OAuth2 access tokens, as
well as handle API key for access token exchanges using the OAuth2 client
credentials grant type.

**NOTE**: In the future we might need to accept options regarding which grant
types this authenticator should support.


#### Methods

This section lists the methods that this class should have.

| Name | Description | Input | Output | REST | Java | Python | Node | PHP | Ruby |
| ---- | ----------- | ----- | ------ | ---- | ---- | ------ | ---- | --- | ---- |
| Authenticate | A method used to authenticate an HTTP Request object | OAuth Authentication Request | OAuth Authentication Result |  |  |  |  |  |  |


### OAuthBearerRequestAuthenticator

This class should authenticate OAuth2 bearer token requests only.  It will only
look for the bearer token in the HTTP request.


#### Methods

This section lists the methods that this class should have.

| Name | Description | Input | Output | REST | Java | Python | Node | PHP | Ruby |
| ---- | ----------- | ----- | ------ | ---- | ---- | ------ | ---- | --- | ---- |
| Authenticate | A method used to authenticate an HTTP Request object | OAuth Bearer Authentication Request| OAuth Bearer Authentication Result |  |  |  |  |  |  |


### OAuthClientCredentialsRequestAuthenticator

This class should authenticate OAuth2 client credentials grant type requests
only.  It will handle authenticating a request based on API key credentials.


#### Methods

This section lists the methods that this class should have.

| Name | Description | Input | Output | REST | Java | Python | Node | PHP | Ruby |
| ---- | ----------- | ----- | ------ | ---- | ---- | ------ | ---- | --- | ---- |
| Authenticate | A method used to authenticate an HTTP Request object | OAuth Client Credentials Authentication Request| OAuth Client Credentials Authentication Result |  |  |  |  |  |  |

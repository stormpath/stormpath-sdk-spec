# API Auth

This spec references how our Stormpath libraries handle API authentication.

Stormpath provides ways to authenticate API requests using both HTTP Basic
Authentication and OAuth2.


## Authenticators

This section discusses the various authenticator classes that each SDK should
provide.  These classes should be initialized with a Stormpath Application and
accept an HTTP request object of some sort.


### ApiKeyAuthenticator

This class should authenticate HTTP Basic Auth requests.  Meaning that this is usually used when your HTTP Request has the Basic Authorization scheme.  The Basic Authentication Request object MUST take ApiKeyId and ApiKeySecret field.  The Basic Authentication Result MUST return an object with an account and api key.

#### Methods

This section lists the methods that this class should have.

| Name | Description | Input | Output | 
| ---- | ----------- | ----- | ------ | 
| Authenticate | A method used to authenticate an HTTP Request object | Basic Authentication Request| Basic Authentication Result | 

### UsernamePasswordAuthenticator

This class should authenticate HTTP Basic Auth requests.  Meaning that this is usually used when your HTTP Request has the Basic Authorization scheme.  The Basic Authentication Request object should take username and password field.

#### Methods

This section lists the methods that this class should have.

| Name | Description | Input | Output | 
| ---- | ----------- | ----- | ------ | 
| Authenticate | A method used to authenticate an HTTP Request object | Basic Authentication Request| Basic Authentication Result | 

### PasswordOAuthAuthenticator

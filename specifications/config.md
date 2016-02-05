# Configuration

This document specifies how Stormpath SDKs should handle configuration.

## Processing

The order in which the configuration should be processed.

1. Load configuration according to *loading order*.
2. Check and apply any *post-processing keys*.
3. Validate the configuration according to the *validation rules*.

## Loading order

The order in which the configuration should be loaded.

1. Default configuration.
2. `apiKey.properties` file from `~/.stormpath` directory**.
3. `stormpath.[json or yaml]` file from `~/.stormpath` directory**.
4. `apiKey.properties` file from application directory**.
5. `stormpath.[json or yaml]` file from application directory**.
6. Environment variables.
7. Configuration provided through the SDK client constructor. I.e for the Node.js SDK: `new stormpath.Client({/* config here */});`.

***Ignore file if empty or doesn't exist.*


## Default configuration

The **default** configuration, represented as [YAML](https://en.wikipedia.org/wiki/YAML):

```yaml
---
client:
  apiKey:
    file: null
    id: null
    secret: null
  cacheManager:
    defaultTtl: 300
    defaultTti: 300
    caches: { }
  baseUrl: "https://api.stormpath.com/v1"
  connectionTimeout: 30
  authenticationScheme: "SAUTHC1"
  proxy:
    port: null
    host: null
    username: null
    password: null
application:
  name: null
  href: null
```

> :bulb: See the [stormpath.yaml](#stormpathjsonyaml) section below for examples of properties not specified in the default configuration.

## Files

### apiKey.properties

This file descibes a Stormpath API key using the [.properties file format](https://en.wikipedia.org/wiki/.properties).
The file can be present in both the `~/.stormpath` directory and the application directory.
Loading should fail if either `apiKey.id` or `apiKey.secret` isn't present.
If the file exists but is empty it should be ignored.

```
apiKey.id = 4VAAAAAAAAAAAAAAAAAAAAAAX4
apiKey.secret = tnAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAPE
```

### stormpath.[json/yaml]

This file describes an SDK configuration file in either [json](https://en.wikipedia.org/wiki/JSON) or [yaml](https://en.wikipedia.org/wiki/YAML) file format.
The file can be present in both the `~/.stormpath` directory and the application directory.

```yaml
---
client:
  apiKey:
    file: null
    id: null
    secret: null
  cacheManager:
    defaultTtl: 300
    defaultTti: 300
    caches:
      # Resource cache configurations, as needed.
      # These are used to override the default TTL/TTI settings
      # for a particular resource type.
      account:
        ttl: 300
        tti: 300
      
  baseUrl: "https://api.stormpath.com/v1"
  connectionTimeout: 30
  authenticationScheme: "SAUTHC1"
  proxy:
    port: null
    host: null
    username: null
    password: null
application:
  name: null
  href: null
```

## Environment variables

Configuration can be loaded from the environment by keys prefixed with `STORMPATH_`.
Keys should be formatted in uppercase with underscore separating each key/sub key. I.e. `STORMPATH_CLIENT_APIKEY_ID` or `STORMPATH_APPLICATION_HREF`.
Stormpath environment keys are represented as a flattened version of the otherwise deep configuration object. I.e. `STORMPATH_CLIENT_APIKEY_ID` actually represents the key `client.apiKey.id`.

The process of mapping configuration from environment variables should be the following:

1. Load the *default configuration*.
2. Walk all of its keys.
3. For each key that doesn't have a value with sub keys, flatten the key and format it as a Stormpath environment variable. I.e. key `client.apiKey.id` should be flattened into `STORMPATH_CLIENT_APIKEY_ID`. Then, if that environment variable exist, then map the value of that environment variable to the configuration key.

## Post-processing keys

Certain configuration keys can be used in order to determine specific post-processing behaviour.

### client.apiKey.file

If the key `client.apiKey.file` isn't empty, then a `apiKey.properties` file should be loaded from that path.
Unlike from the other `apiKey.properties` files, if this file doesn't exists then an error should be thrown.

### apiKey

If an `apiKey` key is set, then this key should be mapped to the key `client.apiKey`. This behaviour exists purely because of backward compatibility.

## Validation

### API key

1. Check if the key `client.apiKey` exists. If the key is missing then throw an error stating `API key cannot be empty`.
2. Check if either key `client.apiKey.id` or `client.apiKey.secret` are missing. If either one of them are, then throw an error stating `API key ID and secret is required`.

### Application

1. Check if the key `application.href` exists.
2. If the key exists, then validate that the value contains the substring `/applications/`. This is just a simple sanity check to see if the HREF provided is a valid Stormpath application HREF. If the value doesn't contain that string, then throw an error stating `'(the invalid HREF)' is not a valid Stormpath Application HREF.`.

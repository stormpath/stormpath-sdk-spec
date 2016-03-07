# User Agent

This document specifies how Stormpath SDKs should handle and report the `User-Agent` header.

## Behavior

All SDKs must report the information about the current running environment back to Stormpath. This reporting will use the HTTP `User-Agent` header. The header format is a series of  whitespace-delimited token groups:

```
[integration token(s)?] [SDK token] [runtime token] [OS token] (anything else?)
```

The sections below will describe each token group. An complete SDK `User-Agent` string will look like the following:

```
stormpath-sdk-angularjs/0.9.0 angular/1.4.7 express-stormpath/3.0.0 express/4.13.4 stormpath-sdk-node/0.17.5 node/5.7.1 Windows/6.3
|-------------------------integration tokens-------------------------------------| |--------SDK token------| |-runtime| |-OS token|
```

## Extensibility

The SDK must provide a mechanism for framework integrations built on top of the SDK to add additional framework and [agent tracking](https://github.com/stormpath/stormpath-framework-spec/blob/master/agent-tracking.md) tokens to a given request. The additional tokens provided by the integration layer should be **prepended** to the base SDK user agent string.

Ideally, the SDK should allow the base Client to be scoped for a set of related requests, so that the additional tokens will be applied to each scoped request, but not necessarily subsequent requests. For now, this requirement is not mandatory.

At minimum, the SDK should make a best attempt to send the tokens provided by the integration layer along with the appropriate request(s).

## Token Groups

#### Integration Tokens *(if any)*

A string containing tokens provided by the integration layer, as described in Extensibility above.

This should be omitted if null or empty (if there is no integration being used).

#### SDK Token

The Stormpath SDK name and version separated by a slash '/'.

#### Runtime Token

The low-level runtime that is hosting the SDK. For example, `python/2.2.3`.

#### OS Token

The machine's operating system name and version. For example, `Windows/6.3`.

#### Anything Else *(if any)*

All other system information should be included in parenthesis.

This should be omitted if null or empty.

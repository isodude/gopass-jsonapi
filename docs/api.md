## API Overview

The API follows the specification for native messaging from [Mozilla](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Native_messaging) and [Chrome](https://developer.chrome.com/apps/nativeMessaging).
Each JSON-UTF8 encoded message is prefixed with a 32-bit integer specifying the length of the message.
Communication is performed via stdin/stdout.

**WARNING**: This API **MUST NOT** be exposed over the network to remote hosts.
**No authentication is performed** and the only safe way is to communicate via stdin/stdout as you do in your terminal.

## Request Types

### `query`

#### Query:

```json
{
  "type": "query",
  "query": "secret"
}
```

#### Response:

```json
[
    "somewhere/mysecret/loginname",
    "somewhere/else/secretsauce"
]
```

### `queryHost`

Similar to `query` but cuts hostnames and subdomains from the left side until the response to the query is non-empty. Stops if only the [public suffix](https://publicsuffix.org/) is remaining.

#### Query:

```json
{
  "type": "queryHost",
  "host": "some.domain.example.com"
}
```

#### Response:

```json
[
    "somewhere/domain.example.com/loginname",
    "somewhere/other.domain.example.com"
]
```

### `getLogin`

#### Query:

```json
{
   "type": "getLogin",
   "entry": "somewhere/else/secretsauce"
}
```

#### Response:

```json
{
   "username": "hugo",
   "password": "thepassword"
}
```

### `getData`

#### Query:

```json
{
   "type": "getData",
   "entry": "somewhere/else/secretsauce"
}
```

#### Response:

```json
{
   "current_totp": "576548"
}
```

### `create`

#### Query:

```json
{
   "type": "create",
   "login": "myusername",
   "password": "",
   "length": 12,
   "generate": true,
   "use_symbols": true
}
```

#### Response:

```json
{
   "username": "myusername",
   "password": "5^dX9j1\"b5^q"
}
```

## Error Response

If an uncaught error occurs, the stringified error message is send back as the response:

```json
{
  "error": "Some error occurred with fancy message"
}
```

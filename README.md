# Scoops on Wheels TypeScript Library

[![fern shield](https://img.shields.io/badge/%F0%9F%8C%BF-Built%20with%20Fern-brightgreen)](https://buildwithfern.com?utm_source=github&utm_medium=github&utm_campaign=readme&utm_source=https%3A%2F%2Fgithub.com%2Fdvdaruri%2Fscoops-on-wheels-typescript-sdk)
[![npm shield](https://img.shields.io/npm/v/scoops-on-wheels-dvdaruri)](https://www.npmjs.com/package/scoops-on-wheels-dvdaruri)

Scoops on Wheels is a fleet of ice cream trucks across New York City.
This SDK lets you find the nearest truck, browse menus, and filter by
price, flavor, and availability — all in a few lines of code.


## Table of Contents

- [Documentation](#documentation)
- [Installation](#installation)
- [Reference](#reference)
- [About The Fleet](#about-the-fleet)
- [Quick Example](#quick-example)
- [Usage](#usage)
- [Request and Response Types](#request-and-response-types)
- [Exception Handling](#exception-handling)
- [Advanced](#advanced)
  - [Subpackage Exports](#subpackage-exports)
  - [Additional Headers](#additional-headers)
  - [Additional Query String Parameters](#additional-query-string-parameters)
  - [Retries](#retries)
  - [Timeouts](#timeouts)
  - [Aborting Requests](#aborting-requests)
  - [Access Raw Response Data](#access-raw-response-data)
  - [Logging](#logging)
  - [Custom Fetch](#custom-fetch)
  - [Runtime Compatibility](#runtime-compatibility)

## Documentation

API reference documentation is available [here](https://github.com/dvdaruri/scoops-on-wheels-api).

## Installation

```sh
npm i -s scoops-on-wheels-dvdaruri
```

## Reference

A full reference for this library is available [here](https://github.com/dvdaruri/scoops-on-wheels-typescript-sdk/blob/HEAD/./reference.md).

## About the Fleet

Scoops on Wheels operates 5 trucks across NYC neighborhoods including
Midtown, Brooklyn, Queens, the Upper West Side, and the Bronx.
All trucks are updated in real-time with live location and menu availability.


## Quick Example

Find open trucks near you under $5:

```typescript
const client = new ScoopsOnWheels();

const results = await client.trucks.searchNearby({
  latitude: 40.7549,
  longitude: -73.9840,
  radiusMiles: 1.5,
  maxPrice: 5,
  isOpen: true,
});

console.log(`Found ${results.total} trucks nearby!`);
```


## Usage

Instantiate and use the client with the following:

```typescript
import { ScoopsOnWheelsClient } from "scoops-on-wheels-dvdaruri";

const client = new ScoopsOnWheelsClient({ environment: "YOUR_BASE_URL" });
await client.search.searchNearby({
    latitude: 1.1,
    longitude: 1.1
});
```

## Request and Response Types

The SDK exports all request and response types as TypeScript interfaces. Simply import them with the
following namespace:

```typescript
import { ScoopsOnWheels } from "scoops-on-wheels-dvdaruri";

const request: ScoopsOnWheels.GetTruckTrucksTruckIdGetRequest = {
    ...
};
```

## Exception Handling

When the API returns a non-success status code (4xx or 5xx response), a subclass of the following error
will be thrown.

```typescript
import { ScoopsOnWheelsError } from "scoops-on-wheels-dvdaruri";

try {
    await client.search.searchNearby(...);
} catch (err) {
    if (err instanceof ScoopsOnWheelsError) {
        console.log(err.statusCode);
        console.log(err.message);
        console.log(err.body);
        console.log(err.rawResponse);
    }
}
```

## Advanced

### Subpackage Exports

This SDK supports direct imports of subpackage clients, which allows JavaScript bundlers to tree-shake and include only the imported subpackage code. This results in much smaller bundle sizes.

```typescript
import { TrucksClient } from 'scoops-on-wheels-dvdaruri/trucks';

const client = new TrucksClient({...});
```

### Additional Headers

If you would like to send additional headers as part of the request, use the `headers` request option.

```typescript
import { ScoopsOnWheelsClient } from "scoops-on-wheels-dvdaruri";

const client = new ScoopsOnWheelsClient({
    ...
    headers: {
        'X-Custom-Header': 'custom value'
    }
});

const response = await client.search.searchNearby(..., {
    headers: {
        'X-Custom-Header': 'custom value'
    }
});
```

### Additional Query String Parameters

If you would like to send additional query string parameters as part of the request, use the `queryParams` request option.

```typescript
const response = await client.search.searchNearby(..., {
    queryParams: {
        'customQueryParamKey': 'custom query param value'
    }
});
```

### Retries

The SDK is instrumented with automatic retries with exponential backoff. A request will be retried as long
as the request is deemed retryable and the number of retry attempts has not grown larger than the configured
retry limit (default: 2).

Which status codes are retried depends on the `retryStatusCodes` generator configuration:

**`legacy`** (current default): retries on
- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [5XX](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#server_error_responses) (All server errors, including 500)

**`recommended`**: retries on
- [408](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/408) (Timeout)
- [429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) (Too Many Requests)
- [502](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/502) (Bad Gateway)
- [503](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/503) (Service Unavailable)
- [504](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/504) (Gateway Timeout)

Use the `maxRetries` request option to configure this behavior.

```typescript
const response = await client.search.searchNearby(..., {
    maxRetries: 0 // override maxRetries at the request level
});
```

### Timeouts

The SDK defaults to a 60 second timeout. Use the `timeoutInSeconds` option to configure this behavior.

```typescript
const response = await client.search.searchNearby(..., {
    timeoutInSeconds: 30 // override timeout to 30s
});
```

### Aborting Requests

The SDK allows users to abort requests at any point by passing in an abort signal.

```typescript
const controller = new AbortController();
const response = await client.search.searchNearby(..., {
    abortSignal: controller.signal
});
controller.abort(); // aborts the request
```

### Access Raw Response Data

The SDK provides access to raw response data, including headers, through the `.withRawResponse()` method.
The `.withRawResponse()` method returns a promise that results to an object with a `data` and a `rawResponse` property.

```typescript
const { data, rawResponse } = await client.search.searchNearby(...).withRawResponse();

console.log(data);
console.log(rawResponse.headers['X-My-Header']);
```

### Logging

The SDK supports logging. You can configure the logger by passing in a `logging` object to the client options.

```typescript
import { ScoopsOnWheelsClient, logging } from "scoops-on-wheels-dvdaruri";

const client = new ScoopsOnWheelsClient({
    ...
    logging: {
        level: logging.LogLevel.Debug, // defaults to logging.LogLevel.Info
        logger: new logging.ConsoleLogger(), // defaults to ConsoleLogger
        silent: false, // defaults to true, set to false to enable logging
    }
});
```
The `logging` object can have the following properties:
- `level`: The log level to use. Defaults to `logging.LogLevel.Info`.
- `logger`: The logger to use. Defaults to a `logging.ConsoleLogger`.
- `silent`: Whether to silence the logger. Defaults to `true`.

The `level` property can be one of the following values:
- `logging.LogLevel.Debug`
- `logging.LogLevel.Info`
- `logging.LogLevel.Warn`
- `logging.LogLevel.Error`

To provide a custom logger, you can pass in an object that implements the `logging.ILogger` interface.

<details>
<summary>Custom logger examples</summary>

Here's an example using the popular `winston` logging library.
```ts
import winston from 'winston';

const winstonLogger = winston.createLogger({...});

const logger: logging.ILogger = {
    debug: (msg, ...args) => winstonLogger.debug(msg, ...args),
    info: (msg, ...args) => winstonLogger.info(msg, ...args),
    warn: (msg, ...args) => winstonLogger.warn(msg, ...args),
    error: (msg, ...args) => winstonLogger.error(msg, ...args),
};
```

Here's an example using the popular `pino` logging library.

```ts
import pino from 'pino';

const pinoLogger = pino({...});

const logger: logging.ILogger = {
  debug: (msg, ...args) => pinoLogger.debug(args, msg),
  info: (msg, ...args) => pinoLogger.info(args, msg),
  warn: (msg, ...args) => pinoLogger.warn(args, msg),
  error: (msg, ...args) => pinoLogger.error(args, msg),
};
```
</details>


### Custom Fetch

The SDK provides a low-level `fetch` method for making custom HTTP requests while still
benefiting from SDK-level configuration like authentication, retries, timeouts, and logging.
This is useful for calling API endpoints not yet supported in the SDK.

```typescript
const response = await client.fetch("/v1/custom/endpoint", {
    method: "GET",
}, {
    timeoutInSeconds: 30,
    maxRetries: 3,
    headers: {
        "X-Custom-Header": "custom-value",
    },
});

const data = await response.json();
```

### Runtime Compatibility


The SDK works in the following runtimes:



- Node.js 18+
- Vercel
- Cloudflare Workers
- Deno v1.25+
- Bun 1.0+
- React Native



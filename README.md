
# Getting Started with OneThing API

## Introduction

OneThing turns a photo, a link or a picked product into a live one-product shop that takes
PayPal payments, or a recommendation page that earns referral commissions through Channel3.

**How a shop is made**

1. `createShop`: start from a photo, a link, or a Channel3 product. Gemini guesses what it
   is, how it's delivered, the price, a short link and the page design. You get back the
   shop's link name and an **admin token**.
2. `updateShop`: correct anything the AI guessed, then set `status: live`.
3. Buyers pay on `https://<host>/<slug>` with PayPal. Sellers read orders and ship them.

**Auth.** Seller endpoints take the shop's admin token in the `x-admin-token` header, or a
**platform API key** (`Authorization: Bearer ot_sk_…`) for marketplaces that make shops for
their own sellers. Buyer endpoints (checkout, access-code verification, downloads) are public.

**Access codes.** A digital product can unlock an app or game instead of shipping a file.
Each paid order gets a code such as `K7QF-M2XP-9HTD`; your app calls `verifyAccessCode`
before unlocking. Raptor Rush on miriogames.com is a live example.

**Agent-ready.** Every live shop is also published for AI shopping agents: an MCP server at
`/api/mcp` with `search_shops`, `get_product`, `create_order` and `get_order`, and the same
four as plain GETs for assistants that can only fetch URLs (`listAgentShops`,
`getAgentProduct`, `orderByLink`, `getAgentOrder`), with `/llms.txt` as the starting point.
Order and status links also have short forms, `/<slug>/buy` and `/o/<order_id>/status`.
An agent can find and order, but the buyer still approves every payment at PayPal.

**Limits.** Calls that cost something are capped per network: 120 checkouts per 10 minutes,
10 new shops an hour and 60 AI requests an hour. Over the cap the reply is a 429 with
`Retry-After`.

All money moves through PayPal (sandbox in this deployment). OneThing never holds card data.

## Install the Package

Run the following command from your project directory to install the package from npm:

```bash
npm install one-thing-sdk@1.0.0
```

For additional package details, see the [Npm page for the one-thing-sdk@1.0.0 npm](https://www.npmjs.com/package/one-thing-sdk/v/1.0.0).

## Initialize the API Client

**_Note:_** Documentation for the client can be found [here.](doc/client.md)

The following parameters are configurable for the API Client:

| Parameter | Type | Description |
|  --- | --- | --- |
| environment | [`Environment`](README.md#environments) | The API environment. <br> **Default: `Environment.Production`** |
| timeout | `number` | Timeout for API calls.<br>*Default*: `60000` |
| httpClientOptions | [`Partial<HttpClientOptions>`](doc/http-client-options.md) | Stable configurable http client options. |
| unstableHttpClientOptions | `any` | Unstable configurable http client options. |
| adminTokenCredentials | [`AdminTokenCredentials`](doc/auth/custom-header-signature.md) | The credential object for adminToken |
| platformKeyCredentials | [`PlatformKeyCredentials`](doc/auth/oauth-2-bearer-token.md) | The credential object for platformKey |

The API client can be initialized as follows:

### Code-Based Client Initialization

```ts
import { Client, Environment } from 'one-thing-sdk';

const client = new Client({
  adminTokenCredentials: {
    'x-admin-token': 'x-admin-token'
  },
  platformKeyCredentials: {
    accessToken: 'AccessToken'
  },
  timeout: 60000,
  environment: Environment.Production,
});
```

### Configuration-Based Client Initialization

```ts
import * as path from 'path';
import * as fs from 'fs';
import { Client } from 'one-thing-sdk';

// Provide absolute path for the configuration file
const absolutePath = path.resolve('./config.json');

// Read the configuration file content
const fileContent = fs.readFileSync(absolutePath, 'utf-8');

// Initialize client from JSON configuration content
const client = Client.fromJsonConfig(fileContent);
```

See the [Configuration-Based Client Initialization](doc/configuration-based-client-initialization.md) section for details.

### Environment-Based Client Initialization

```ts
import * as dotenv from 'dotenv';
import * as path from 'path';
import * as fs from 'fs';
import { Client } from 'one-thing-sdk';

// Optional - Provide absolute path for the .env file
const absolutePath = path.resolve('./.env');

if (fs.existsSync(absolutePath)) {
  // Load environment variables from .env file
  dotenv.config({ path: absolutePath, override: true });
}

// Initialize client using environment variables
const client = Client.fromEnvironment(process.env);
```

See the [Environment-Based Client Initialization](doc/environment-based-client-initialization.md) section for details.

## Environments

The SDK can be configured to use a different environment for making API calls. Available environments are:

### Fields

| Name | Description |
|  --- | --- |
| Production | **Default** Hackathon deployment (PayPal sandbox) |
| Local | Local development |

## Authorization

This API uses the following authentication schemes.

* [`AdminToken (Custom Header Signature)`](doc/auth/custom-header-signature.md)
* [`PlatformKey (OAuth 2 Bearer token)`](doc/auth/oauth-2-bearer-token.md)

## List of APIs

* [Accesscodes](doc/controllers/accesscodes.md)
* [Accounts](doc/controllers/accounts.md)
* [Shops](doc/controllers/shops.md)
* [Links](doc/controllers/links.md)
* [Products](doc/controllers/products.md)
* [Checkout](doc/controllers/checkout.md)
* [Orders](doc/controllers/orders.md)
* [Files](doc/controllers/files.md)
* [Agents](doc/controllers/agents.md)
* [Platforms](doc/controllers/platforms.md)

## Webhooks

* [Webhooks](doc/events/webhooks/webhooks-handler.md)

## SDK Infrastructure

### Configuration

* [HttpClientOptions](doc/http-client-options.md)
* [RetryConfiguration](doc/retry-configuration.md)
* [ProxySettings](doc/proxy-settings.md)
* [Configuration-Based Client Initialization](doc/configuration-based-client-initialization.md)
* [Environment-Based Client Initialization](doc/environment-based-client-initialization.md)

### HTTP

* [HttpRequest](doc/http-request.md)

### Utilities

* [ApiResponse](doc/api-response.md)
* [ApiError](doc/api-error.md)


# Agents

Machine-readable shops and the MCP server AI shopping agents buy through.

```ts
const agentsApi = new AgentsApi(client);
```

## Class Name

`AgentsApi`

## Methods

* [Open Pay Link](../../doc/controllers/agents.md#open-pay-link)
* [List Agent Shops](../../doc/controllers/agents.md#list-agent-shops)
* [Order by Link](../../doc/controllers/agents.md#order-by-link)
* [Get Agent Order](../../doc/controllers/agents.md#get-agent-order)
* [Get Agent Product](../../doc/controllers/agents.md#get-agent-product)
* [Call Mcp](../../doc/controllers/agents.md#call-mcp)


# Open Pay Link

The `pay_url` an agent gives the buyer. It redirects to PayPal's payment page for the order, so PayPal's checkout token never passes through the agent (agent platforms hide such tokens as secrets). A paid order redirects to its thank-you page; an expired checkout goes back to the shop page.

:information_source: **Note** This endpoint does not require authentication.

```ts
async openPayLink(
  orderId: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<void>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `orderId` | `string` | Template, Required | The `order_id` from the MCP `create_order` tool. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**

This method returns an [`ApiResponse`](../../doc/api-response.md) instance.

## Example Usage

```ts
const orderId = 'pX4kR9tVb2nQ7sLe';

try {
  const response = await agentsApi.openPayLink(orderId);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
  }
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No order with that id. | `ApiError` |


# List Agent Shops

The gallery's live shops for a query: shops matching more of its words first, newest first among equals. Browsing words ("something interesting to buy") are ignored, and a query that matches nothing returns the newest shops with `matched: false` rather than an empty list. The GET twin of the MCP `search_shops` tool, for agents that can only fetch URLs. Public. Read a shop with `getAgentProduct`; buy through the MCP server in `mcp_server`, or send the buyer to the shop's `url`.

:information_source: **Note** This endpoint does not require authentication.

```ts
async listAgentShops(
  q?: string,
  limit?: number,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiAgentShopsResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `q` | `string \| undefined` | Query, Optional | Keywords. Leave out to list the newest shops.<br><br>**Constraints**: *Maximum Length*: `200` |
| `limit` | `number \| undefined` | Query, Optional | How many shops, 1–25.<br><br>**Default**: `10`<br><br>**Constraints**: `>= 1`, `<= 25` |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The matching shops.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiAgentShopsResponse`](../../doc/models/api-agent-shops-response.md).

## Example Usage

```ts
const q = 'skates';

const limit = 10;

try {
  const response = await agentsApi.listAgentShops(
    q,
    limit
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
  }
}
```

## Example Response *(as JSON)*

```json
{
  "show_buyer": "![Vintage Cream and Red Roller Skates](https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X)\n\n**Vintage Cream and Red Roller Skates**: $240.00. Classic quad skates crafted with walnut burl heels\n\n![Raptor Rush: Browser Edition](https://one-thing-ash0.onrender.com/api/blobs/R9tWc3yN6bJq1zUd)\n\n**Raptor Rush: Browser Edition**: $2.99. Grow from Tiny Hatchling to Giant Apex Predator\n\nWhich one?",
  "shops": [
    {
      "shop": "vintageskates",
      "name": "Vintage Cream and Red Roller Skates",
      "kind": "physical",
      "headline": "Classic quad skates crafted with walnut burl heels",
      "price": "$240.00",
      "image": "https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X",
      "url": "https://one-thing-ash0.onrender.com/vintageskates"
    },
    {
      "shop": "raptorrush",
      "name": "Raptor Rush: Browser Edition",
      "kind": "digital",
      "headline": "Grow from Tiny Hatchling to Giant Apex Predator",
      "price": "$2.99",
      "image": "https://one-thing-ash0.onrender.com/api/blobs/R9tWc3yN6bJq1zUd",
      "url": "https://one-thing-ash0.onrender.com/raptorrush"
    }
  ],
  "matched": true,
  "mcp_server": "https://one-thing-ash0.onrender.com/api/mcp"
}
```


# Order by Link

Orders one unit with a GET: the MCP `create_order` tool for AI assistants that can only
fetch URLs, such as free-tier ChatGPT or Grok. Nothing is charged. The reply has `pay_url`,
where the buyer pays at PayPal, and `status_url` (`getAgentOrder`), which says when they
have and what they got.

For a product with options, pass the buyer's pick as query parameters named after the
options (`?Size=9`, or `?Color=Ice+Blue&Size=L`); names and values are matched without
regard to case. You rarely build this URL yourself: `getAgentProduct` and `/llms.txt`
write it out in full (`order_url`, or `variants[].order_url`), because some assistants
only open URLs they have been shown.

Every request makes a new order, so open it once, after the buyer says to buy. Known
crawlers and link previewers get a 403 (and are kept out by `robots.txt`); any other
leaves an unpaid order at most, since stock is only taken when a payment is captured.
Replies are `no-store` and `X-Robots-Tag: noindex, nofollow`.

`/<slug>/buy` is the same endpoint at a short URL, and the one `order_url` uses.
A person clicking the link in a browser (`Sec-Fetch-User: ?1`) is redirected instead; known assistants and headless browsers always get JSON. It goes straight to `pay_url`, or to the shop page when an option is missing
or unknown.

:information_source: **Note** This endpoint does not require authentication.

```ts
async orderByLink(
  slug: string,
  agent?: string,
  size?: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<AgentCreatedOrder>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `agent` | `string \| undefined` | Query, Optional | Optional: the agent's name (`ChatGPT`, `Gemini`, `my-shopping-bot`), so the seller's stats show which agent sent the buyer. Only used when the user agent doesn't already say.<br><br>**Constraints**: *Maximum Length*: `30` |
| `size` | `string \| undefined` | Query, Optional | An example option. The real parameter names are the product's option names from `getAgentProduct` (`options[].name`), one per option. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The order, waiting for the buyer to pay.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`AgentCreatedOrder`](../../doc/models/agent-created-order.md).

## Example Usage

```ts
const slug = 'raptorrush';

const agent = 'ChatGPT';

const size = '9';

try {
  const response = await agentsApi.orderByLink(
    slug,
    agent,
    size
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "show_buyer": "![Vintage Cream and Red Roller Skates](https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X)\n\nVintage Cream and Red Roller Skates (9): $255.00 total.\n**[Pay $255.00 with PayPal](https://one-thing-ash0.onrender.com/o/pX4kR9tVb2nQ7sLe)**\nTell me once you've paid and I'll get you the shipping confirmation.",
  "order_id": "pX4kR9tVb2nQ7sLe",
  "status": "awaiting_buyer_approval",
  "total": "$255.00",
  "options": {
    "Size": "9"
  },
  "pay_url": "https://one-thing-ash0.onrender.com/o/pX4kR9tVb2nQ7sLe",
  "status_url": "https://one-thing-ash0.onrender.com/o/pX4kR9tVb2nQ7sLe/status",
  "next_step": "Show pay_url to the buyer as a link. It opens PayPal, where they pay (and enter a shipping address for physical items). When they say they've paid, open status_url (or with MCP, call get_order with order_id): it returns the access code, download link or shipping details."
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | A missing or unknown option value, or sold out. `error` names the valid values. | [`CustomError`](../../doc/models/custom-error.md) |
| 403 | A known crawler or link previewer. They don't place orders. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No live shop with that link name. | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |


# Get Agent Order

Where an order stands and, once paid, what the buyer got: the access code, the download link, or the shipping address and tracking. The GET twin of the MCP `get_order` tool (and the `status_url` that `create_order` returns), for agents that can only fetch URLs. Public: anyone with the order id can already open its thank-you page. If the buyer has approved at PayPal but nothing has captured the payment yet, this captures it. `/o/<order_id>/status` is the same endpoint at a short URL, and the one `status_url` uses. A person clicking the link in a browser (`Sec-Fetch-User: ?1`) is redirected instead; known assistants and headless browsers always get JSON. It goes to the order's receipt page.

:information_source: **Note** This endpoint does not require authentication.

```ts
async getAgentOrder(
  orderId: string,
  agent?: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<AgentOrder>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `orderId` | `string` | Template, Required | The `order_id` from the MCP `create_order` tool, or from a thank-you page link (`?order=`). |
| `agent` | `string \| undefined` | Query, Optional | Optional: the agent's name (`ChatGPT`, `Gemini`, `my-shopping-bot`), so the seller's stats show which agent sent the buyer. Only used when the user agent doesn't already say.<br><br>**Constraints**: *Maximum Length*: `30` |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The order.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`AgentOrder`](../../doc/models/agent-order.md).

## Example Usage

```ts
const orderId = 'pX4kR9tVb2nQ7sLe';

const agent = 'ChatGPT';

try {
  const response = await agentsApi.getAgentOrder(
    orderId,
    agent
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "order_id": "pX4kR9tVb2nQ7sLe",
  "status": "awaiting_buyer_approval",
  "product": "Vintage Cream and Red Roller Skates",
  "options": {
    "Size": "9"
  },
  "amount": {
    "item": 240,
    "shipping": 15,
    "total": 255,
    "currency": "USD"
  },
  "total": "$255.00",
  "pay_url": "https://one-thing-ash0.onrender.com/o/pX4kR9tVb2nQ7sLe",
  "next_step": "The buyer hasn't paid yet. Give them pay_url; when they say they've paid, call get_order again."
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No order with that id. | [`CustomError`](../../doc/models/custom-error.md) |


# Get Agent Product

Everything an AI shopping agent needs to describe the product and decide to buy it: price, shipping, delivery, availability, options, images and FAQ. Public. Buying goes through the MCP server named in `mcp_server`, or the `order_url` links. Shop pages also carry the same facts as schema.org `Product` JSON-LD. A person clicking the link in a browser (`Sec-Fetch-User: ?1`) is redirected instead; known assistants and headless browsers always get JSON. It goes to the shop page, unless the link has `?view=json`.

:information_source: **Note** This endpoint does not require authentication.

```ts
async getAgentProduct(
  slug: string,
  agent?: string,
  view?: View,
  requestOptions?: RequestOptions
): Promise<ApiResponse<AgentProduct>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `agent` | `string \| undefined` | Query, Optional | Optional: the agent's name (`ChatGPT`, `Gemini`, `my-shopping-bot`), so the seller's stats show which agent sent the buyer. Only used when the user agent doesn't already say.<br><br>**Constraints**: *Maximum Length*: `30` |
| `view` | [`View \| undefined`](../../doc/models/view.md) | Query, Optional | `json` keeps the JSON for a person opening the link in a browser. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The product.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`AgentProduct`](../../doc/models/agent-product.md).

## Example Usage

```ts
const slug = 'raptorrush';

const agent = 'ChatGPT';

const view = View.Json;

try {
  const response = await agentsApi.getAgentProduct(
    slug,
    agent,
    view
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "show_buyer": "![Vintage Cream and Red Roller Skates](https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X)\n\n**Vintage Cream and Red Roller Skates**: $240.00 + $15.00 shipping\nClassic quad skates crafted with walnut burl heels\nSize: 7 · 8 · 9\nWhich size?",
  "shop": "vintageskates",
  "url": "https://one-thing-ash0.onrender.com/vintageskates",
  "name": "Vintage Cream and Red Roller Skates",
  "kind": "physical",
  "headline": "Classic quad skates crafted with walnut burl heels",
  "description": "A vintage pair of skates featuring cream leather, bold red trim, and durable quad wheels.",
  "highlights": [
    "Handmade cream leather upper with rich red trim",
    "Sculpted walnut burl heel structure for stability"
  ],
  "details": [
    {
      "label": "Materials",
      "value": "Leather upper, walnut burl heel, rubber wheels"
    }
  ],
  "faq": [
    {
      "q": "What is the return policy?",
      "a": "Unworn skates in original condition can be returned within 30 days of delivery."
    }
  ],
  "images": [
    "https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X",
    "https://one-thing-ash0.onrender.com/api/blobs/zrySajNyao6_j11x"
  ],
  "price": {
    "amount": 240,
    "currency": "USD",
    "label": "$240.00"
  },
  "shipping": {
    "amount": 15,
    "currency": "USD",
    "ships_to": [
      "US"
    ]
  },
  "total": {
    "amount": 255,
    "currency": "USD"
  },
  "delivery": "Shipped. PayPal collects the buyer's address at checkout.",
  "options": [
    {
      "name": "Size",
      "values": [
        "7",
        "8",
        "9"
      ]
    }
  ],
  "variants": [
    {
      "choice": {
        "Size": "7"
      },
      "availability": "in_stock (3 left)",
      "order_url": "https://one-thing-ash0.onrender.com/vintageskates/buy?Size=7"
    },
    {
      "choice": {
        "Size": "8"
      },
      "availability": "sold_out"
    },
    {
      "choice": {
        "Size": "9"
      },
      "availability": "in_stock (6 left)",
      "order_url": "https://one-thing-ash0.onrender.com/vintageskates/buy?Size=9"
    }
  ],
  "availability": "in_stock",
  "how_to_buy": "Show show_buyer to the buyer exactly as written, photo included (it's Markdown, so the photo displays and the links stay clickable), and keep your own words to a minimum. Once the buyer has picked and said to buy, open the order_url of their variant (or call create_order on the OneThing MCP server with shop and options). Either returns show_buyer with the pay link.",
  "mcp_server": "https://one-thing-ash0.onrender.com/api/mcp",
  "agent_name": "ChatGPT"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No live shop with that link name. | [`CustomError`](../../doc/models/custom-error.md) |


# Call Mcp

A [Model Context Protocol](https://modelcontextprotocol.io) server, so agents such as
Claude can shop here. Add `https://<host>/api/mcp` to an MCP client as a Streamable HTTP
server. It's stateless: each POST carries one JSON-RPC 2.0 message (or a batch) and gets a
plain JSON reply, with no event stream. The `initialize` reply's `Mcp-Session-Id` header
carries the client's `clientInfo.name`; send it back on later requests (MCP clients do)
so the seller's stats show which agent sent the buyer. Nothing else is kept.

Methods: `initialize`, `ping`, `tools/list`, `tools/call`, and `resources/list`,
`resources/templates/list` and `resources/read` for the product card. Each tool carries
`annotations` (only `create_order` isn't `readOnlyHint`). Tools:

| Tool | Arguments | What it does |
|---|---|---|
| `search_shops` | `query?`, `limit?` (1–25) | Live gallery shops, best match first; no match returns the newest with `matched: false` |
| `get_product` | `shop` | The same object as `getAgentProduct` |
| `create_order` | `shop`, `options?` | One unit on order; returns `order_id` and `pay_url` |
| `get_order` | `order_id` | Status and `next_step`; once paid, the receipt and `delivery` |

**The buyer always approves.** `create_order` charges nothing: the agent shows the buyer
`pay_url` (`/o/<order_id>`, see `openPayLink`), which opens PayPal. It's a plain link on this
site, so agent platforms that hide PayPal's checkout tokens can still show it. When the buyer
says they've paid, the agent calls `get_order`.

`get_order` status is one of `awaiting_buyer_approval`, `paid`, `declined` (nothing charged;
the buyer can open `pay_url` again), `sold_out`, `expired` (call `create_order` again) or
`failed` (PayPal couldn't be reached; try again), with a `next_step` the agent can follow.
A paid order carries `delivery`: `{type: access_code, access_code, start_url?}`,
`{type: download, download_url}` or `{type: shipping, ship_to, fulfillment, tracking}`, plus
`amount` with item, shipping and total. Orders made this way show `channel: agent` in
`listOrders`.

**The product card.** Hosts that draw MCP Apps UI (ChatGPT, or a client that declares the
`io.modelcontextprotocol/ui` extension) get `search_shops`, `get_product` and
`create_order` results as a card with the real photos, options and a Pay with PayPal button
(`ui://onething/shop-card-v1.html`). For them `show_buyer` is a one-line note rather than
Markdown, so the model doesn't repeat or invent what the card shows.

Tool errors (an unknown shop, a sold-out size, over the checkout cap) come back as results
with `isError: true`, so the model can read them and try something else.

:information_source: **Note** This endpoint does not require authentication.

```ts
async callMcp(
  body: CallMcpBody,
  requestOptions?: RequestOptions
): Promise<ApiResponse<CallMcpResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `body` | [`CallMcpBody`](../../doc/models/containers/call-mcp-body.md) | Body, Required | This is a container for one-of cases. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The JSON-RPC reply (an array for a batch).

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type `CallMcpResponse`.

## Example Usage

```ts
const body: CallMcpBody = {
  jsonrpc: '2.0',
  method: Method.EnumToolscall,
  id: 1,
  params: { 'name': 'search_shops', 'arguments': { 'query': 'game' } },
};

try {
  const response = await agentsApi.callMcp(body);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof JsonRpcResponseError) {
      console.log(error.result);
    }
  }
}
```

## Example Response

```
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "content": [
      {
        "type": "text",
        "text": "{\n  \"show_buyer\": \"![Vintage Cream and Red Roller Skates](https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X)\\n\\n**Vintage Cream and Red Roller Skates**: $240.00. Classic quad skates crafted with walnut burl heels\\n\\nWant it?\",\n  \"shops\": [\n    {\n      \"shop\": \"vintageskates\",\n      \"name\": \"Vintage Cream and Red Roller Skates\",\n      \"kind\": \"physical\",\n      \"headline\": \"Classic quad skates crafted with walnut burl heels\",\n      \"price\": \"$240.00\",\n      \"image\": \"https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X\",\n      \"url\": \"https://one-thing-ash0.onrender.com/vintageskates\"\n    }\n  ],\n  \"agent_name\": \"Claude\"\n}"
      }
    ],
    "structuredContent": {
      "show_buyer": "![Vintage Cream and Red Roller Skates](https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X)\n\n**Vintage Cream and Red Roller Skates**: $240.00. Classic quad skates crafted with walnut burl heels\n\nWant it?",
      "shops": [
        {
          "shop": "vintageskates",
          "name": "Vintage Cream and Red Roller Skates",
          "kind": "physical",
          "headline": "Classic quad skates crafted with walnut burl heels",
          "price": "$240.00",
          "image": "https://one-thing-ash0.onrender.com/api/blobs/LFRvGuTU3D8i334X",
          "url": "https://one-thing-ash0.onrender.com/vintageskates"
        }
      ],
      "agent_name": "Claude"
    },
    "isError": false
  }
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Not JSON (JSON-RPC parse error `-32700`). | [`JsonRpcResponseError`](../../doc/models/json-rpc-response-error.md) |


# Orders

The seller's orders, fulfilment and PayPal webhook log.

```ts
const ordersApi = new OrdersApi(client);
```

## Class Name

`OrdersApi`

## Methods

* [List Orders](../../doc/controllers/orders.md#list-orders)
* [Mark Order Shipped](../../doc/controllers/orders.md#mark-order-shipped)
* [Refund Order](../../doc/controllers/orders.md#refund-order)
* [List Webhook Events](../../doc/controllers/orders.md#list-webhook-events)


# List Orders

Newest first. Checkouts that were never approved are left out.

```ts
async listOrders(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<Order[]>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Orders.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`Order[]`](../../doc/models/order.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await ordersApi.listOrders(slug);

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
[
  {
    "id": "pX4kR9tVb2nQ7sLe",
    "paypal_order_id": "5O190127TN364715T",
    "status": "paid",
    "amount_cents": 25500,
    "shipping_cents": 1500,
    "currency": "USD",
    "buyer_email": "buyer@example.com",
    "buyer_name": "Sam Rivera",
    "shipping_address": {
      "address_line_1": "1 Main St",
      "admin_area_2": "Springfield",
      "admin_area_1": "IL",
      "postal_code": "62701",
      "country_code": "US"
    },
    "capture_id": "3C679366HH908993F",
    "fulfillment_status": "to_ship",
    "carrier": null,
    "tracking_number": null,
    "tracking_synced": false,
    "downloads": 0,
    "access_code": null,
    "code_uses": 0,
    "channel": "agent",
    "options": {
      "Size": "8"
    },
    "paid_at": "2026-10-22T02:41:37Z",
    "created_at": "2026-10-22T02:38:05Z"
  }
]
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Mark Order Shipped

Saves the tracking number and pushes it to PayPal, so the buyer sees it in PayPal.

```ts
async markOrderShipped(
  slug: string,
  orderId: string,
  body: ApiShopsOrdersShipRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsOrdersShipResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `orderId` | `string` | Template, Required | OneThing's order id (`Order.id`). |
| `body` | [`ApiShopsOrdersShipRequest`](../../doc/models/api-shops-orders-ship-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Shipped.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsOrdersShipResponse`](../../doc/models/api-shops-orders-ship-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const orderId = 'pX4kR9tVb2nQ7sLe';

const body: ApiShopsOrdersShipRequest = {
  carrier: Carrier.Usps,
  trackingNumber: '9400111899223817362315',
};

try {
  const response = await ordersApi.markOrderShipped(
    slug,
    orderId,
    body
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
  "ok": true,
  "synced": true
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Refund Order

Refunds the whole payment through PayPal and marks the order `refunded`. When PayPal only lets the paid account refund it (the money went to a PayPal email the seller typed in, not a connected account), the reply is a 409 with `refundInPayPal: true`. Refund it in your PayPal account, then call this again with `{"record": true}`, which only marks it refunded here. Sends the `order.refunded` event to the shop's Zapier hook.

```ts
async refundOrder(
  slug: string,
  orderId: string,
  body?: ApiShopsOrdersRefundRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsOrdersRefundResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `orderId` | `string` | Template, Required | OneThing's order id (`Order.id`). |
| `body` | [`ApiShopsOrdersRefundRequest \| undefined`](../../doc/models/api-shops-orders-refund-request.md) | Body, Optional | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Refunded.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsOrdersRefundResponse`](../../doc/models/api-shops-orders-refund-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const orderId = 'pX4kR9tVb2nQ7sLe';

const body: ApiShopsOrdersRefundRequest = {
};

try {
  const response = await ordersApi.refundOrder(
    slug,
    orderId,
    body
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
    } else if (error instanceof ApiShopsOrdersRefund409Error) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "ok": true,
  "refundId": "1JU08902781691411"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop or order, or the token doesn't match the shop. | [`CustomError`](../../doc/models/custom-error.md) |
| 409 | Not refundable here; `error` says why. | [`ApiShopsOrdersRefund409Error`](../../doc/models/api-shops-orders-refund-409-error.md) |


# List Webhook Events

The last 50 PayPal webhook deliveries about this shop's orders, with signature results.

```ts
async listWebhookEvents(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<WebhookEvent[]>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Events, newest first.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`WebhookEvent[]`](../../doc/models/webhook-event.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await ordersApi.listWebhookEvents(slug);

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
[
  {
    "id": "WH-2WR32451HC0233532-67976317FL4543714",
    "event_type": "PAYMENT.CAPTURE.COMPLETED",
    "paypal_order_id": "5O190127TN364715T",
    "summary": "255.00 USD",
    "verified": true,
    "outcome": "Confirmed: order already paid",
    "received_at": "2026-10-22T02:41:40Z"
  }
]
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


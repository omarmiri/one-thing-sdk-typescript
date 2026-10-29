# Checkout

Buyer-side PayPal checkout.

```ts
const checkoutApi = new CheckoutApi(client);
```

## Class Name

`CheckoutApi`

## Methods

* [Get Pay Pal Client Token](../../doc/controllers/checkout.md#get-pay-pal-client-token)
* [Create Checkout Order](../../doc/controllers/checkout.md#create-checkout-order)
* [Capture Checkout Order](../../doc/controllers/checkout.md#capture-checkout-order)


# Get Pay Pal Client Token

A browser-safe client token for the PayPal JavaScript SDK v6
(`paypal.createInstance({ clientToken })`), bound to the requesting site's domain.
Shop pages call this before showing the PayPal button.

:information_source: **Note** This endpoint does not require authentication.

```ts
async getPayPalClientToken(
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiPaypalClientTokenResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: A token for this site's domain.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiPaypalClientTokenResponse`](../../doc/models/api-paypal-client-token-response.md).

## Example Usage

```ts
try {
  const response = await checkoutApi.getPayPalClientToken();

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
  "clientToken": "eyJraWQiOiJFWEFNUExFIn0.example.client-token"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |
| 503 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Create Checkout Order

Creates the PayPal order for one unit of a live shop's product. The amount always comes
from the shop, never from the caller. Pass the returned `id` to the PayPal JS SDK.

If the shop has buyer `options`, send one value for each. A sold-out combination answers 409.

:information_source: **Note** This endpoint does not require authentication.

```ts
async createCheckoutOrder(
  body: ApiOrdersRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiOrdersResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `body` | [`ApiOrdersRequest`](../../doc/models/api-orders-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: PayPal order created.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiOrdersResponse`](../../doc/models/api-orders-response.md).

## Example Usage

```ts
const body: ApiOrdersRequest = {
  slug: 'raptorrush',
};

try {
  const response = await checkoutApi.createCheckoutOrder(body);

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
  "id": "5O190127TN364715T"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 409 | Taken or unavailable (a link name, or a sold-out product). | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |
| 503 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Capture Checkout Order

Call after the buyer approves. Records the sale; digital orders get a download link or access code.
Safe to call repeatedly: before approval it answers 409, so a page can poll it while the PayPal window is open.

:information_source: **Note** This endpoint does not require authentication.

```ts
async captureCheckoutOrder(
  paypalOrderId: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiOrdersCaptureResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `paypalOrderId` | `string` | Template, Required | The PayPal order id that `createCheckoutOrder` returned. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Paid.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiOrdersCaptureResponse`](../../doc/models/api-orders-capture-response.md).

## Example Usage

```ts
const paypalOrderId = '5O190127TN364715T';

try {
  const response = await checkoutApi.captureCheckoutOrder(paypalOrderId);

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
    } else if (error instanceof ApiOrdersCapture409Error) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "orderId": "pX4kR9tVb2nQ7sLe",
  "status": "paid"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 402 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 409 | The buyer hasn't approved the order yet. | [`ApiOrdersCapture409Error`](../../doc/models/api-orders-capture-409-error.md) |
| 410 | The last unit (or the last of the buyer's combination) sold while they were paying, or the PayPal checkout expired before they paid. The payment isn't captured, so the buyer isn't charged. | [`CustomError`](../../doc/models/custom-error.md) |
| 422 | The buyer's funding source was declined (`INSTRUMENT_DECLINED`); they can try another. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


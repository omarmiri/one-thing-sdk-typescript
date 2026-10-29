# Accounts

Sign in as a seller with an email or a phone number. Shops made while signed in belong to the account.

```ts
const accountsApi = new AccountsApi(client);
```

## Class Name

`AccountsApi`

## Methods

* [Sign In](../../doc/controllers/accounts.md#sign-in)
* [Sign Out](../../doc/controllers/accounts.md#sign-out)
* [Log In with Pay Pal](../../doc/controllers/accounts.md#log-in-with-pay-pal)


# Sign In

Signs in with an email address or a phone number, creating the account the first time. There's no password and no verification (sandbox). The response sets the `ot_session` cookie; shops created with it belong to the account and are listed on `/account`. Signing in with an email also claims older shops whose seller email matches. In a browser, sellers can instead use Log in with PayPal (`/api/account/paypal`), which also records the PayPal account their sales are paid into.

:information_source: **Note** This endpoint does not require authentication.

```ts
async signIn(
  body: ApiAccountSigninRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiAccountSigninResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `body` | [`ApiAccountSigninRequest`](../../doc/models/api-account-signin-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Signed in. The `ot_session` cookie is set.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiAccountSigninResponse`](../../doc/models/api-account-signin-response.md).

## Example Usage

```ts
const body: ApiAccountSigninRequest = {
  identifier: 'maker@example.com',
};

try {
  const response = await accountsApi.signIn(body);

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
  "id": "Vq3nN8cT0bLx2YwE",
  "email": "maker@example.com",
  "phone": null
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |


# Sign Out

Ends this browser's session and clears the `ot_session` cookie.

:information_source: **Note** This endpoint does not require authentication.

```ts
async signOut(requestOptions?: RequestOptions): Promise<ApiResponse<Ok>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Done.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`Ok`](../../doc/models/ok.md).

## Example Usage

```ts
try {
  const response = await accountsApi.signOut();

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
  "ok": true
}
```


# Log In with Pay Pal

A link for a browser, not for code: redirects to PayPal's sign-in page. PayPal sends the seller back to `/api/account/paypal/callback`, which signs them in (creating the account the first time), records the PayPal account their sales are paid into, and redirects to `next`.

:information_source: **Note** This endpoint does not require authentication.

```ts
async logInWithPayPal(
  next?: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<void>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `next` | `string \| undefined` | Query, Optional | A path on this site to land on afterwards. Defaults to `/account`. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**

This method returns an [`ApiResponse`](../../doc/api-response.md) instance.

## Example Usage

```ts
const next = '/new';

try {
  const response = await accountsApi.logInWithPayPal(next);

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

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 503 | Log in with PayPal isn't switched on for this server. | [`CustomError`](../../doc/models/custom-error.md) |


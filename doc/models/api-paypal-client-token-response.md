
# Api Paypal Client Token Response

The client token.

## Structure

`ApiPaypalClientTokenResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `clientToken` | `string` | Required | Pass to `createInstance` in the PayPal JavaScript SDK v6. |

## Example

```ts
import { ApiPaypalClientTokenResponse } from 'one-thing-sdk';

const apiPaypalClientTokenResponse: ApiPaypalClientTokenResponse = {
  clientToken: 'eyJraWQiOiJFWEFNUExFIn0.example.client-token',
};
```


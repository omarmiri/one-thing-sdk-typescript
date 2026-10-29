
# Api Shops Paypal Request

Where to come back to.

## Structure

`ApiShopsPaypalRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `returnUrl` | `string \| undefined` | Optional | Platform key only. Where to send the seller after PayPal. |

## Example

```ts
import { ApiShopsPaypalRequest } from 'one-thing-sdk';

const apiShopsPaypalRequest: ApiShopsPaypalRequest = {
  returnUrl: 'https://www.miriogames.com/market/manage/?g=7Hq2',
};
```


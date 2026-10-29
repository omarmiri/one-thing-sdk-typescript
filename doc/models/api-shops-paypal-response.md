
# Api Shops Paypal Response

The onboarding link.

## Structure

`ApiShopsPaypalResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `url` | `string` | Required | PayPal's onboarding page for this shop. |

## Example

```ts
import { ApiShopsPaypalResponse } from 'one-thing-sdk';

const apiShopsPaypalResponse: ApiShopsPaypalResponse = {
  url: 'https://www.sandbox.paypal.com/bizsignup/partner/entry?referralToken=EXAMPLE',
};
```


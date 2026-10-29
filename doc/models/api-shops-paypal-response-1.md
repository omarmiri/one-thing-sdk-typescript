
# Api Shops Paypal Response 1

The shop's PayPal connection.

## Structure

`ApiShopsPaypalResponse1`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `merchantId` | `string \| null` | Required | The seller's PayPal merchant id. |
| `status` | [`ApiShopsPaypalResponse1Status \| null`](../../doc/models/containers/api-shops-paypal-response-1-status.md) | Required | This is a container for one-of cases. |

## Example

```ts
import { ApiShopsPaypalResponse1 } from 'one-thing-sdk';

const apiShopsPaypalResponse1: ApiShopsPaypalResponse1 = {
  merchantId: '7MZJ4QXN8K2LA',
  status: {
    receivable: true,
    emailConfirmed: true,
    checkedAt: '2026-10-22T02:41:37Z',
  },
};
```


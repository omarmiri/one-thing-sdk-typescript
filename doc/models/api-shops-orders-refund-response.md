
# Api Shops Orders Refund Response

## Structure

`ApiShopsOrdersRefundResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | - |
| `refundId` | `string \| null \| undefined` | Optional | PayPal's refund id; null when `record` was used. |

## Example

```ts
import { ApiShopsOrdersRefundResponse } from 'one-thing-sdk';

const apiShopsOrdersRefundResponse: ApiShopsOrdersRefundResponse = {
  ok: false,
  refundId: 'refundId6',
};
```


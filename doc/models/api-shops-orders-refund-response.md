
# Api Shops Orders Refund Response

The refund.

## Structure

`ApiShopsOrdersRefundResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | Always `true`. |
| `refundId` | `string \| null \| undefined` | Optional | PayPal's refund id; null when `record` was used. |

## Example

```ts
import { ApiShopsOrdersRefundResponse } from 'one-thing-sdk';

const apiShopsOrdersRefundResponse: ApiShopsOrdersRefundResponse = {
  ok: true,
  refundId: '1JU08902781691411',
};
```


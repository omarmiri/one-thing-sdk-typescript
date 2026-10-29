
# Api Shops Orders Refund Request

## Structure

`ApiShopsOrdersRefundRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `record` | `boolean \| undefined` | Optional | `true`: don't contact PayPal, just record that the seller refunded it themselves.<br><br>**Default**: `false` |

## Example

```ts
import { ApiShopsOrdersRefundRequest } from 'one-thing-sdk';

const apiShopsOrdersRefundRequest: ApiShopsOrdersRefundRequest = {
  record: false,
};
```


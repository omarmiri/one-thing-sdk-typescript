
# Api Shops Orders Ship Request

## Structure

`ApiShopsOrdersShipRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `carrier` | [`Carrier`](../../doc/models/carrier.md) | Required | - |
| `carrierName` | `string \| undefined` | Optional | Required when `carrier` is `OTHER`.<br><br>**Constraints**: *Maximum Length*: `60` |
| `trackingNumber` | `string` | Required | **Constraints**: *Minimum Length*: `4`, *Maximum Length*: `64` |

## Example

```ts
import { ApiShopsOrdersShipRequest, Carrier } from 'one-thing-sdk';

const apiShopsOrdersShipRequest: ApiShopsOrdersShipRequest = {
  carrier: Carrier.Dhl,
  trackingNumber: 'trackingNumber4',
  carrierName: 'carrierName4',
};
```


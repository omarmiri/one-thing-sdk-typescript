
# Api Shops Orders Ship Request

The shipment's tracking.

## Structure

`ApiShopsOrdersShipRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `carrier` | [`Carrier`](../../doc/models/carrier.md) | Required | The carrier. `OTHER` needs `carrierName`. |
| `carrierName` | `string \| undefined` | Optional | Required when `carrier` is `OTHER`.<br><br>**Constraints**: *Maximum Length*: `60` |
| `trackingNumber` | `string` | Required | The carrier's tracking number.<br><br>**Constraints**: *Minimum Length*: `4`, *Maximum Length*: `64` |

## Example

```ts
import { ApiShopsOrdersShipRequest, Carrier } from 'one-thing-sdk';

const apiShopsOrdersShipRequest: ApiShopsOrdersShipRequest = {
  carrier: Carrier.Usps,
  trackingNumber: '9400111899223197428490',
  carrierName: 'Canada Post',
};
```


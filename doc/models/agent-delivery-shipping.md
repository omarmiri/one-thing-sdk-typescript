
# Agent Delivery Shipping

A physical product shipped to the buyer.

## Structure

`AgentDeliveryShipping`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `type` | `string` | Required, Constant | Always `shipping`.<br><br>**Value**: `'shipping'` |
| `shipTo` | [`ShipTo \| null \| undefined`](../../doc/models/ship-to.md) | Optional | Where it ships, from the buyer's PayPal checkout. Null if PayPal didn't share it. |
| `fulfillment` | [`Fulfillment`](../../doc/models/fulfillment.md) | Required | `to_ship` until the seller adds tracking, then `shipped`. |
| `tracking` | [`Tracking \| null \| undefined`](../../doc/models/tracking.md) | Optional | The carrier and tracking number once shipped; null before. |

## Example

```ts
import { AgentDeliveryShipping, Fulfillment } from 'one-thing-sdk';

const agentDeliveryShipping: AgentDeliveryShipping = {
  type: 'shipping',
  fulfillment: Fulfillment.ToShip,
  shipTo: {
    name: 'Sam Rivera',
    address: '1 Main St, Springfield, IL, 62701, US',
  },
  tracking: {
    carrier: 'USPS',
    number: '9400111899223197428490',
  },
};
```


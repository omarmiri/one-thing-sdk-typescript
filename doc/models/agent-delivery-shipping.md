
# Agent Delivery Shipping

A physical product shipped to the buyer.

## Structure

`AgentDeliveryShipping`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `type` | `string` | Required, Constant | **Value**: `'shipping'` |
| `shipTo` | [`ShipTo \| null \| undefined`](../../doc/models/ship-to.md) | Optional | - |
| `fulfillment` | [`Fulfillment`](../../doc/models/fulfillment.md) | Required | - |
| `tracking` | [`Tracking \| null \| undefined`](../../doc/models/tracking.md) | Optional | - |

## Example

```ts
import { AgentDeliveryShipping, Fulfillment } from 'one-thing-sdk';

const agentDeliveryShipping: AgentDeliveryShipping = {
  type: 'shipping',
  fulfillment: Fulfillment.ToShip,
  shipTo: {
    name: 'name6',
    address: 'address2',
  },
  tracking: {
    carrier: 'carrier2',
    number: 'number8',
  },
};
```


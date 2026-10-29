
# Order Event

What OneThing posts to the seller's Zapier hook.

## Structure

`OrderEvent`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `event` | [`Event`](../../doc/models/event.md) | Required | What happened. |
| `shop` | [`Shop1`](../../doc/models/shop-1.md) | Required | The shop the order belongs to. |
| `order` | [`Order1`](../../doc/models/order-1.md) | Required | The order. |

## Example

```ts
import { Event, OrderEvent } from 'one-thing-sdk';

const orderEvent: OrderEvent = {
  event: Event.EnumOrderpaid,
  shop: {
    slug: 'vintageskates',
    name: 'Vintage Cream and Red Roller Skates',
    url: 'https://one-thing-ash0.onrender.com/vintageskates',
  },
  order: {
    id: 'pX4kR9tVb2nQ7sLe',
    paypalOrderId: '5O190127TN364715T',
    amount: '255.00',
    currency: 'USD',
    buyerName: 'Sam Rivera',
    buyerEmail: 'sam@example.com',
    shippingAddress: { 'address_line_1': '1 Main St', 'admin_area_2': 'Springfield', 'admin_area_1': 'IL', 'postal_code': '62701', 'country_code': 'US' },
    fulfillmentStatus: 'to_ship',
    trackingNumber: '9400111899223197428490',
    carrier: 'USPS',
  },
};
```


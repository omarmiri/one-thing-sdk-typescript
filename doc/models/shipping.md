
# Shipping

Shipping cost and destinations.

## Structure

`Shipping`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `amount` | `number` | Required | Shipping cost per order. |
| `currency` | `string` | Required | ISO 4217 currency code. |
| `shipsTo` | `string[]` | Required | Country codes; empty means anywhere PayPal ships. |

## Example

```ts
import { Shipping } from 'one-thing-sdk';

const shipping: Shipping = {
  amount: 15,
  currency: 'USD',
  shipsTo: [
    'US',
    'CA'
  ],
};
```


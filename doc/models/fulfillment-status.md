
# Fulfillment Status

`to_ship` and `shipped` for physical orders, `digital_sent` for digital ones, `none` before payment.

## Enumeration

`FulfillmentStatus`

## Fields

| Name |
|  --- |
| `None` |
| `ToShip` |
| `Shipped` |
| `DigitalSent` |

## Example

```ts
import { FulfillmentStatus } from 'one-thing-sdk';

const fulfillmentStatus = FulfillmentStatus.Shipped;
```


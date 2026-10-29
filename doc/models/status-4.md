
# Status 4

`created` until paid; `failed` if PayPal declined; `refunded` after a refund.

## Enumeration

`Status4`

## Fields

| Name |
|  --- |
| `Created` |
| `Paid` |
| `Failed` |
| `Refunded` |

## Example

```ts
import { Status4 } from 'one-thing-sdk';

const status4 = Status4.Failed;
```


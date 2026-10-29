
# Api Shops Orders Ship Response

The order is marked shipped.

## Structure

`ApiShopsOrdersShipResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | Always `true`. |
| `synced` | `boolean` | Required | Whether PayPal accepted the tracking number. |

## Example

```ts
import { ApiShopsOrdersShipResponse } from 'one-thing-sdk';

const apiShopsOrdersShipResponse: ApiShopsOrdersShipResponse = {
  ok: true,
  synced: true,
};
```


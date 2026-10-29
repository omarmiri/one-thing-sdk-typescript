
# Api Shops Orders Ship Response

## Structure

`ApiShopsOrdersShipResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | - |
| `synced` | `boolean` | Required | Whether PayPal accepted the tracking number. |

## Example

```ts
import { ApiShopsOrdersShipResponse } from 'one-thing-sdk';

const apiShopsOrdersShipResponse: ApiShopsOrdersShipResponse = {
  ok: false,
  synced: false,
};
```


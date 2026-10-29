
# Api Shops Response

The shop was saved.

## Structure

`ApiShopsResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | Always `true`. |
| `slug` | `string` | Required | The shop's link name after this change. |

## Example

```ts
import { ApiShopsResponse } from 'one-thing-sdk';

const apiShopsResponse: ApiShopsResponse = {
  ok: true,
  slug: 'vintageskates',
};
```


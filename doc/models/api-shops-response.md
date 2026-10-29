
# Api Shops Response

## Structure

`ApiShopsResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | - |
| `slug` | `string` | Required | The shop's link name after this change. |

## Example

```ts
import { ApiShopsResponse } from 'one-thing-sdk';

const apiShopsResponse: ApiShopsResponse = {
  ok: false,
  slug: 'slug8',
};
```


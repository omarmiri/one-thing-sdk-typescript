
# Api Shops Color Photos Request

Which photo shows each colour.

## Structure

`ApiShopsColorPhotosRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `photos` | `Record<string, string \| null>` | Required | Colour label → photo id, or `null`. |

## Example

```ts
import { ApiShopsColorPhotosRequest } from 'one-thing-sdk';

const apiShopsColorPhotosRequest: ApiShopsColorPhotosRequest = {
  photos: {
    'Ice Blue': 'zrySajNyao6_j11x',
    'Amber': ''
  },
};
```


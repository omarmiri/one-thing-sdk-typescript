
# Api Shops Color Photos Request

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
    'key0': 'photos0'
  },
};
```


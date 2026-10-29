
# Api Shops Color Photos Response

The colours that changed.

## Structure

`ApiShopsColorPhotosResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `saved` | `string[]` | Required | Colour labels whose photo changed. |

## Example

```ts
import { ApiShopsColorPhotosResponse } from 'one-thing-sdk';

const apiShopsColorPhotosResponse: ApiShopsColorPhotosResponse = {
  saved: [
    'Ice Blue',
    'Amber'
  ],
};
```


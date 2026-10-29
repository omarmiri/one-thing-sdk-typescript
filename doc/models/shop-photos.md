
# Shop Photos

The shop's photos.

## Structure

`ShopPhotos`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `photos` | [`Photo[]`](../../doc/models/photo.md) | Required | Every photo, the cover first. |

## Example

```ts
import { ShopPhotos } from 'one-thing-sdk';

const shopPhotos: ShopPhotos = {
  photos: [
    {
      id: 'LFRvGuTU3D8i334X',
      url: '/api/blobs/LFRvGuTU3D8i334X',
    }
  ],
};
```


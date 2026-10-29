
# Api Shops Match Photos Request

## Structure

`ApiShopsMatchPhotosRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `colors` | [`Color[]`](../../doc/models/color.md) | Required | **Constraints**: *Minimum Items*: `1`, *Maximum Items*: `12` |

## Example

```ts
import { ApiShopsMatchPhotosRequest } from 'one-thing-sdk';

const apiShopsMatchPhotosRequest: ApiShopsMatchPhotosRequest = {
  colors: [
    {
      label: 'Forest green',
      hex: '#14171a',
    }
  ],
};
```


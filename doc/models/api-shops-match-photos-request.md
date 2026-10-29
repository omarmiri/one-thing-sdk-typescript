
# Api Shops Match Photos Request

The colours to find photos for.

## Structure

`ApiShopsMatchPhotosRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `colors` | [`Color[]`](../../doc/models/color.md) | Required | The colour option's values.<br><br>**Constraints**: *Minimum Items*: `1`, *Maximum Items*: `12` |

## Example

```ts
import { ApiShopsMatchPhotosRequest } from 'one-thing-sdk';

const apiShopsMatchPhotosRequest: ApiShopsMatchPhotosRequest = {
  colors: [
    {
      label: 'Ice Blue',
      hex: '#9fd3e6',
    }
  ],
};
```


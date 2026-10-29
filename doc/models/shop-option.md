
# Shop Option

Something the buyer picks, such as a size or a colour. The shop still sells one product.

## Structure

`ShopOption`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `name` | `string` | Required | **Constraints**: *Maximum Length*: `20` |
| `kind` | [`Kind`](../../doc/models/kind.md) | Required | `color` shows swatches, and the photo of the picked colour. |
| `values` | [`OptionValue[]`](../../doc/models/option-value.md) | Required | **Constraints**: *Minimum Items*: `1`, *Maximum Items*: `12` |

## Example

```ts
import { Kind, ShopOption } from 'one-thing-sdk';

const shopOption: ShopOption = {
  name: 'Size',
  kind: Kind.Size,
  values: [
    {
      label: 'M',
      hex: '#14171a',
      photo: 'photo6',
    }
  ],
};
```


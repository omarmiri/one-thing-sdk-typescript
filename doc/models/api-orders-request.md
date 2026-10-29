
# Api Orders Request

The shop to buy from and the buyer's pick.

## Structure

`ApiOrdersRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Required | The shop's link name. |
| `options` | `Record<string, string> \| undefined` | Optional | The buyer's pick, one value per option, keyed by option name. |

## Example

```ts
import { ApiOrdersRequest } from 'one-thing-sdk';

const apiOrdersRequest: ApiOrdersRequest = {
  slug: 'vintageskates',
  options: {
    'Size': 'M',
    'Color': 'Black'
  },
};
```


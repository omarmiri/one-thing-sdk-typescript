
# Api Access Verify Response

Whether the code is valid.

## Structure

`ApiAccessVerifyResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `valid` | `boolean` | Required | True if the code belongs to a paid, unrefunded order for this shop. |
| `product` | `string \| undefined` | Optional | The product name, when valid. |

## Example

```ts
import { ApiAccessVerifyResponse } from 'one-thing-sdk';

const apiAccessVerifyResponse: ApiAccessVerifyResponse = {
  valid: true,
  product: 'Vintage Cream and Red Roller Skates',
};
```


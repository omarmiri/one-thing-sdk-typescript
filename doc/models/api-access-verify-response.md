
# Api Access Verify Response

## Structure

`ApiAccessVerifyResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `valid` | `boolean` | Required | - |
| `product` | `string \| undefined` | Optional | The product name, when valid. |

## Example

```ts
import { ApiAccessVerifyResponse } from 'one-thing-sdk';

const apiAccessVerifyResponse: ApiAccessVerifyResponse = {
  valid: false,
  product: 'product4',
};
```


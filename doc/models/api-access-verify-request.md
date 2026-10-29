
# Api Access Verify Request

## Structure

`ApiAccessVerifyRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `shop` | `string` | Required | The shop's link name. |
| `code` | `string` | Required | - |

## Example

```ts
import { ApiAccessVerifyRequest } from 'one-thing-sdk';

const apiAccessVerifyRequest: ApiAccessVerifyRequest = {
  shop: 'raptorrush',
  code: 'K7QF-M2XP-9HTD',
};
```


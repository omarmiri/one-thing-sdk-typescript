
# Api Account Signin Request

## Structure

`ApiAccountSigninRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `identifier` | `string` | Required | An email address, or a phone number (spaces, dashes and brackets are ignored; keep a leading +). |

## Example

```ts
import { ApiAccountSigninRequest } from 'one-thing-sdk';

const apiAccountSigninRequest: ApiAccountSigninRequest = {
  identifier: 'maker@example.com',
};
```


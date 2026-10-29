
# Api Slugs Check Response

## Structure

`ApiSlugsCheckResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Required | - |
| `available` | `boolean` | Required | - |
| `problem` | `string \| undefined` | Optional | Why not, when unavailable. |

## Example

```ts
import { ApiSlugsCheckResponse } from 'one-thing-sdk';

const apiSlugsCheckResponse: ApiSlugsCheckResponse = {
  slug: 'slug8',
  available: false,
  problem: 'problem8',
};
```


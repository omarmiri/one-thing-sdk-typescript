
# Api Shops Match Photos Response

Gemini's matches.

## Structure

`ApiShopsMatchPhotosResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `matches` | `Record<string, string>` | Required | Colour label → photo id. |

## Example

```ts
import { ApiShopsMatchPhotosResponse } from 'one-thing-sdk';

const apiShopsMatchPhotosResponse: ApiShopsMatchPhotosResponse = {
  matches: {
    'Black': 'b_8f2k1',
    'Forest green': 'b_q93md'
  },
};
```


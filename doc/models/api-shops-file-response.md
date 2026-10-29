
# Api Shops File Response

The uploaded file.

## Structure

`ApiShopsFileResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `ok` | `boolean` | Required | Always `true`. |
| `filename` | `string` | Required | The file's name, as buyers will download it. |

## Example

```ts
import { ApiShopsFileResponse } from 'one-thing-sdk';

const apiShopsFileResponse: ApiShopsFileResponse = {
  ok: true,
  filename: 'dragon-flight-presets.zip',
};
```


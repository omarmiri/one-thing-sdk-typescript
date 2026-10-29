
# Api Shops Photos Request

## Structure

`ApiShopsPhotosRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `photos` | `string[]` | Required | Photo ids, in order.<br><br>**Constraints**: *Maximum Items*: `8` |

## Example

```ts
import { ApiShopsPhotosRequest } from 'one-thing-sdk';

const apiShopsPhotosRequest: ApiShopsPhotosRequest = {
  photos: [
    'photos0',
    'photos1'
  ],
};
```


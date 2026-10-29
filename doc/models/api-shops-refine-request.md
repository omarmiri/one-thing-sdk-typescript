
# Api Shops Refine Request

The design change, in words.

## Structure

`ApiShopsRefineRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `instruction` | `string` | Required | What to change, e.g. "make it feel more premium".<br><br>**Constraints**: *Maximum Length*: `500` |

## Example

```ts
import { ApiShopsRefineRequest } from 'one-thing-sdk';

const apiShopsRefineRequest: ApiShopsRefineRequest = {
  instruction: 'Make it feel more premium',
};
```


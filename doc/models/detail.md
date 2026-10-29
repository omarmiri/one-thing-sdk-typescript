
# Detail

One product fact.

## Structure

`Detail`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `label` | `string` | Required | What the fact is about. |
| `value` | `string` | Required | The fact. |

## Example

```ts
import { Detail } from 'one-thing-sdk';

const detail: Detail = {
  label: 'Material',
  value: 'Leather upper, walnut burl heel, rubber wheels',
};
```


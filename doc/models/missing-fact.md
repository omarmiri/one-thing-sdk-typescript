
# Missing Fact

## Structure

`MissingFact`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `label` | `string` | Required | - |
| `question` | `string` | Required | - |
| `placeholder` | `string` | Required | - |

## Example

```ts
import { MissingFact } from 'one-thing-sdk';

const missingFact: MissingFact = {
  label: 'Size',
  question: 'What size are the skates?',
  placeholder: 'US women\'s 8',
};
```


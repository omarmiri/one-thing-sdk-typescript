
# Missing Fact

One question to answer.

## Structure

`MissingFact`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `label` | `string` | Required | The product detail it becomes. |
| `question` | `string` | Required | The question, for the seller. |
| `placeholder` | `string` | Required | An example answer. |

## Example

```ts
import { MissingFact } from 'one-thing-sdk';

const missingFact: MissingFact = {
  label: 'Size',
  question: 'What size are the skates?',
  placeholder: 'US women\'s 8',
};
```


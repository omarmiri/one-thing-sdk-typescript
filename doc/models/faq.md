
# Faq

One question and its answer.

## Structure

`Faq`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `q` | `string` | Required | The question. |
| `a` | `string` | Required | The answer. |

## Example

```ts
import { Faq } from 'one-thing-sdk';

const faq: Faq = {
  q: 'Are these suitable for outdoor skating?',
  a: 'Yes, on indoor rink floors and paved outdoor paths.',
};
```


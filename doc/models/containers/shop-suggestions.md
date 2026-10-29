
# Shop Suggestions

## Class Name

`ShopSuggestions`

## Cases

| Type |
|  --- |
| [`Suggestions`](../../../doc/models/suggestions.md) |

## Suggestions

### Initialization Code

#### Example

```ts
const value: ShopSuggestions = {
  oneOfAKind: false,
  options: [
    {
      name: 'Size',
      kind: Kind.Size,
      values: [
        {
          label: 'M',
          hex: '#14171a',
        }
      ],
    }
  ],
  missingFacts: [
    {
      label: 'Size',
      question: 'What size are the skates?',
      placeholder: 'US women\'s 8',
    }
  ],
};
```


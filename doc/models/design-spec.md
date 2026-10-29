
# Design Spec

What Gemini designed. OneThing's templates render it; Gemini never writes HTML.

## Structure

`DesignSpec`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `productType` | [`ProductType2`](../../doc/models/product-type-2.md) | Required | - |
| `productName` | `string` | Required | - |
| `layout` | [`Layout`](../../doc/models/layout.md) | Required | - |
| `palette` | [`Palette`](../../doc/models/palette.md) | Required | - |
| `fonts` | [`Fonts`](../../doc/models/fonts.md) | Required | - |
| `eyebrow` | `string \| undefined` | Optional | - |
| `headline` | `string` | Required | - |
| `subheadline` | `string \| undefined` | Optional | - |
| `story` | `string \| undefined` | Optional | - |
| `bullets` | `string[]` | Required | - |
| `details` | [`Detail[] \| undefined`](../../doc/models/detail.md) | Optional | - |
| `faq` | [`Faq[]`](../../doc/models/faq.md) | Required | - |
| `ctaLabel` | `string` | Required | - |
| `suggestedPriceUsd` | `number \| undefined` | Optional | - |
| `suggestedShippingUsd` | `number \| undefined` | Optional | - |
| `imageAlt` | `string \| undefined` | Optional | - |
| `heroPrompt` | `string \| undefined` | Optional | - |

## Example

```ts
import { DesignSpec, Layout, ProductType2 } from 'one-thing-sdk';

const designSpec: DesignSpec = {
  productType: ProductType2.Digital,
  productName: 'productName0',
  layout: Layout.Boldhero,
  palette: {
    background: '#14171a',
    surface: '#14171a',
    text: '#14171a',
    muted: '#14171a',
    accent: '#14171a',
    accentText: '#14171a',
  },
  fonts: {
    heading: 'heading2',
    body: 'body6',
  },
  headline: 'headline0',
  bullets: [
    'bullets8',
    'bullets9',
    'bullets0'
  ],
  faq: [
    {
      q: 'q0',
      a: 'a8',
    }
  ],
  ctaLabel: 'ctaLabel6',
  eyebrow: 'eyebrow6',
  subheadline: 'subheadline4',
  story: 'story6',
  details: [
    {
      label: 'label0',
      value: 'value2',
    },
    {
      label: 'label0',
      value: 'value2',
    },
    {
      label: 'label0',
      value: 'value2',
    }
  ],
  suggestedPriceUsd: 123.02,
};
```


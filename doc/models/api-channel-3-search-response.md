
# Api Channel 3 Search Response

The products found.

## Structure

`ApiChannel3SearchResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `products` | [`ProductCard[]`](../../doc/models/product-card.md) | Required | Products, best match first. |

## Example

```ts
import { ApiChannel3SearchResponse } from 'one-thing-sdk';

const apiChannel3SearchResponse: ApiChannel3SearchResponse = {
  products: [
    {
      id: 'ch3_7f2a9c1e',
      title: 'Trail Running Vest 5L',
      image: 'https://images.example.com/vest.jpg',
      price: 240,
      currency: 'USD',
      retailer: 'rei.com',
      retailers: 4,
      commission: 0.08,
      brand: 'Salomon',
    }
  ],
};
```


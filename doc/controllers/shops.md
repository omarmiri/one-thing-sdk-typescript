# Shops

Create a shop and manage it as its seller.

```ts
const shopsApi = new ShopsApi(client);
```

## Class Name

`ShopsApi`

## Methods

* [Create Shop](../../doc/controllers/shops.md#create-shop)
* [Get Shop](../../doc/controllers/shops.md#get-shop)
* [Update Shop](../../doc/controllers/shops.md#update-shop)
* [Refine Shop Design](../../doc/controllers/shops.md#refine-shop-design)
* [Regenerate Hero Art](../../doc/controllers/shops.md#regenerate-hero-art)
* [Upload Digital File](../../doc/controllers/shops.md#upload-digital-file)
* [Add Photos](../../doc/controllers/shops.md#add-photos)
* [Set Photo Order](../../doc/controllers/shops.md#set-photo-order)
* [Check Missing Details](../../doc/controllers/shops.md#check-missing-details)
* [Match Color Photos](../../doc/controllers/shops.md#match-color-photos)
* [Set Color Photos](../../doc/controllers/shops.md#set-color-photos)
* [Connect Pay Pal](../../doc/controllers/shops.md#connect-pay-pal)
* [Get Pay Pal Connection](../../doc/controllers/shops.md#get-pay-pal-connection)
* [Disconnect Pay Pal](../../doc/controllers/shops.md#disconnect-pay-pal)


# Create Shop

Starts a draft shop from one of three sources. Gemini designs the page and guesses the
setup; check it with `getShop`, adjust with `updateShop`, then go live.

Sent with a session cookie from `signIn`, the shop belongs to that account, which also
becomes its seller email (and PayPal email, after Log in with PayPal) unless you give one.

- `source=photo`: a `photo` and/or a `description` of your own product.
- `source=link`: a `url`. If Channel3 recognises it as a store's product page, the shop
  becomes a recommendation (`product_type: referral`); otherwise the page is read and you
  sell the product yourself with PayPal.
- `source=channel3`: a `productId` from `searchProducts` (a recommendation).

Takes 15–40 seconds. Hero art, if available, is generated afterwards (`hero_status`).

**Platforms:** sent with a platform API key, the shop belongs to the key's account, so the
key can manage it from then on (no need to keep the admin token). It's made for one of the
platform's sellers, so it inherits nothing from the account: set `paypal_email` or call
`connectPayPal` for that seller.

```ts
async createShop(
  source: Source,
  photo?: FileWrapper,
  description?: string,
  url?: string,
  productId?: string,
  slug?: string,
  type?: Type,
  delivery?: Delivery,
  accessUrl?: string,
  price?: number,
  shipping?: number,
  stock?: number,
  shipCountries?: string,
  paypalEmail?: string,
  sellerEmail?: string,
  digitalFile?: FileWrapper,
  requestOptions?: RequestOptions
): Promise<ApiResponse<CreatedShop>>
```

## Authentication

This endpoint requires [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `source` | [`Source`](../../doc/models/source.md) | Form, Required | - |
| `photo` | `FileWrapper \| undefined` | Form, Optional | `source=photo`: the product photo, up to 8 MB. |
| `description` | `string \| undefined` | Form, Optional | `source=photo`: a sentence or two about the product. Required if there is no photo. |
| `url` | `string \| undefined` | Form, Optional | `source=link`: a product page, or your own app or game page. |
| `productId` | `string \| undefined` | Form, Optional | `source=channel3`: a product id from `searchProducts`. |
| `slug` | `string \| undefined` | Form, Optional | The link name you want. If omitted, the AI suggests one. |
| `type` | [`Type \| undefined`](../../doc/models/type.md) | Form, Optional | **Default**: `Type.Auto` |
| `delivery` | [`Delivery \| undefined`](../../doc/models/delivery.md) | Form, Optional | For digital products. If omitted, the AI guesses. |
| `accessUrl` | `string \| undefined` | Form, Optional | Where buyers use an access code. |
| `price` | `number \| undefined` | Form, Optional | USD. If omitted, the AI suggests a price. |
| `shipping` | `number \| undefined` | Form, Optional | - |
| `stock` | `number \| undefined` | Form, Optional | - |
| `shipCountries` | `string \| undefined` | Form, Optional | - |
| `paypalEmail` | `string \| undefined` | Form, Optional | - |
| `sellerEmail` | `string \| undefined` | Form, Optional | - |
| `digitalFile` | `FileWrapper \| undefined` | Form, Optional | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Draft created.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`CreatedShop`](../../doc/models/created-shop.md).

## Example Usage

```ts
const source = Source.Photo;

const description = 'Vintage cream and red quad roller skates with walnut burl heels.';

const type = Type.Physical;

const price = 240;

const shipping = 15;

const shipCountries = 'US';

const paypalEmail = 'seller@example.com';

try {
  const response = await shopsApi.createShop(
    source,
    undefined,
    description,
    undefined,
    undefined,
    undefined,
    type,
    undefined,
    undefined,
    price,
    shipping,
    undefined,
    shipCountries,
    paypalEmail
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "slug": "vintageskates",
  "token": "3fJ9xQm2LwP8vR1tZk7cN4bY6dHs0aEu",
  "manageUrl": "/manage/vintageskates?token=3fJ9xQm2LwP8vR1tZk7cN4bY6dHs0aEu"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 409 | Taken or unavailable (a link name, or a sold-out product). | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Get Shop

Everything about the shop, including the AI's design spec and guesses.

```ts
async getShop(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<Shop>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The shop.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`Shop`](../../doc/models/shop.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await shopsApi.getShop(slug);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "id": "Hn2c8QvL0xTzR4pW",
  "slug": "vintageskates",
  "status": "live",
  "product_type": "physical",
  "price_cents": 24000,
  "currency": "USD",
  "shipping_cents": 1500,
  "ship_countries": "US",
  "stock": null,
  "delivery": "file",
  "access_url": null,
  "paypal_email": "seller@example.com",
  "seller_email": "seller@example.com",
  "paypal_merchant_id": "7MZJ4QXN8K2LA",
  "paypal_status": {
    "receivable": true,
    "emailConfirmed": true,
    "checkedAt": "2026-10-21T18:04:11Z"
  },
  "zapier_hook_url": null,
  "in_gallery": true,
  "account_id": "Vq3nN8cT0bLx2YwE",
  "options": [
    {
      "name": "Size",
      "kind": "size",
      "values": [
        {
          "label": "7"
        },
        {
          "label": "8"
        },
        {
          "label": "9"
        }
      ]
    }
  ],
  "option_stock": {
    "7": 3,
    "8": 0,
    "9": 6
  },
  "suggestions": null,
  "photo_blob": "LFRvGuTU3D8i334X",
  "gallery_blobs": [
    "zrySajNyao6_j11x",
    "SuqcgNv64HWN8H6e"
  ],
  "hero_blob": null,
  "hero_status": "none",
  "hero_error": null,
  "ai_source": "gemini",
  "source_url": null,
  "referral": null,
  "spec": {
    "productType": "physical",
    "productName": "Vintage Cream and Red Roller Skates",
    "layout": "editorial",
    "palette": {
      "background": "#f6efe4",
      "surface": "#fffaf2",
      "text": "#2b1d14",
      "muted": "#7a6455",
      "accent": "#b3261e",
      "accentText": "#ffffff"
    },
    "fonts": {
      "heading": "Playfair Display",
      "body": "Inter"
    },
    "eyebrow": "One pair, restored",
    "headline": "Classic quad skates crafted with walnut burl heels",
    "subheadline": "A vintage pair of skates featuring cream leather, bold red trim, and durable quad wheels.",
    "story": "Each pair combines soft cream leather with distinct red accents and a sculpted walnut burl heel base.",
    "bullets": [
      "Handmade cream leather upper with rich red trim",
      "Sculpted walnut burl heel structure for stability"
    ],
    "details": [
      {
        "label": "Materials",
        "value": "Leather upper, walnut burl heel, rubber wheels"
      }
    ],
    "faq": [
      {
        "q": "Are these suitable for outdoor skating?",
        "a": "Yes, on indoor rink floors and paved outdoor paths."
      }
    ],
    "ctaLabel": "Buy the skates",
    "suggestedPriceUsd": 240,
    "suggestedShippingUsd": 15,
    "imageAlt": "Cream leather quad roller skates with red trim"
  },
  "created_at": "2026-10-19T15:22:08Z"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Update Shop

Changes only the fields you send. Renaming `slug` keeps the old link working (it
redirects). Set `status: live` to publish; a digital product needs its file (or, for
access codes, `access_url`) first.

```ts
async updateShop(
  slug: string,
  body: ShopUpdate,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `body` | [`ShopUpdate`](../../doc/models/shop-update.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Saved.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsResponse`](../../doc/models/api-shops-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const body: ShopUpdate = {
  status: Status.Live,
};

try {
  const response = await shopsApi.updateShop(
    slug,
    body
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "ok": true,
  "slug": "vintageskates"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Refine Shop Design

"Make it warmer", "shorter copy", "more premium". Gemini edits the design spec.

```ts
async refineShopDesign(
  slug: string,
  body: ApiShopsRefineRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsRefineResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `body` | [`ApiShopsRefineRequest`](../../doc/models/api-shops-refine-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The new design.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsRefineResponse`](../../doc/models/api-shops-refine-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const body: ApiShopsRefineRequest = {
  instruction: 'Make it feel more premium',
};

try {
  const response = await shopsApi.refineShopDesign(
    slug,
    body
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "spec": {
    "productType": "physical",
    "productName": "Vintage Cream and Red Roller Skates",
    "layout": "editorial",
    "palette": {
      "background": "#f6efe4",
      "surface": "#fffaf2",
      "text": "#2b1d14",
      "muted": "#7a6455",
      "accent": "#b3261e",
      "accentText": "#ffffff"
    },
    "fonts": {
      "heading": "Playfair Display",
      "body": "Inter"
    },
    "eyebrow": "One pair, restored",
    "headline": "Classic quad skates crafted with walnut burl heels",
    "subheadline": "A vintage pair of skates featuring cream leather, bold red trim, and durable quad wheels.",
    "story": "Each pair combines soft cream leather with distinct red accents and a sculpted walnut burl heel base.",
    "bullets": [
      "Handmade cream leather upper with rich red trim",
      "Sculpted walnut burl heel structure for stability"
    ],
    "details": [
      {
        "label": "Materials",
        "value": "Leather upper, walnut burl heel, rubber wheels"
      }
    ],
    "faq": [
      {
        "q": "Are these suitable for outdoor skating?",
        "a": "Yes, on indoor rink floors and paved outdoor paths."
      }
    ],
    "ctaLabel": "Buy the skates",
    "suggestedPriceUsd": 240,
    "suggestedShippingUsd": 15,
    "imageAlt": "Cream leather quad roller skates with red trim"
  }
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Regenerate Hero Art

Starts generation in the background. Poll `getShop` for `hero_status`.

```ts
async regenerateHeroArt(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<Ok>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Done.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`Ok`](../../doc/models/ok.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await shopsApi.regenerateHeroArt(slug);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "ok": true
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |


# Upload Digital File

Uploads or replaces a digital product's file, up to 25 MB. Each paid order gets a download link for it.

```ts
async uploadDigitalFile(
  slug: string,
  file: FileWrapper,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsFileResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `file` | `FileWrapper` | Form, Required | Up to 25 MB. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Uploaded.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsFileResponse`](../../doc/models/api-shops-file-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const file = new FileWrapper(fs.createReadStream('dummy_file'));

try {
  const response = await shopsApi.uploadDigitalFile(
    slug,
    file
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "ok": true,
  "filename": "raptor-rush-soundtrack.zip"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Add Photos

Adds one or more photos after the cover. Each is turned upright, resized to at most 2000 px and stored as WebP. A shop holds up to 8 photos; buyers swipe through them on the shop page. Not for recommendations, which show the retailer's photos.

```ts
async addPhotos(
  slug: string,
  photo: FileWrapper,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ShopPhotos>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `photo` | `FileWrapper` | Form, Required | An image, up to 15 MB. The endpoint also accepts the field repeated, to add several in one request. |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The shop's photos, cover first.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ShopPhotos`](../../doc/models/shop-photos.md).

## Example Usage

```ts
const slug = 'raptorrush';

const photo = new FileWrapper(fs.createReadStream('dummy_file'));

try {
  const response = await shopsApi.addPhotos(
    slug,
    photo
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "photos": [
    {
      "id": "LFRvGuTU3D8i334X",
      "url": "/api/blobs/LFRvGuTU3D8i334X"
    },
    {
      "id": "zrySajNyao6_j11x",
      "url": "/api/blobs/zrySajNyao6_j11x"
    }
  ]
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Set Photo Order

Sets the photo order; the first becomes the cover. Leaving a photo out removes it.

```ts
async setPhotoOrder(
  slug: string,
  body: ApiShopsPhotosRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ShopPhotos>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `body` | [`ApiShopsPhotosRequest`](../../doc/models/api-shops-photos-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The shop's photos, cover first.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ShopPhotos`](../../doc/models/shop-photos.md).

## Example Usage

```ts
const slug = 'raptorrush';

const body: ApiShopsPhotosRequest = {
  photos: [
    'zrySajNyao6_j11x',
    'LFRvGuTU3D8i334X',
    'SuqcgNv64HWN8H6e'
  ],
};

try {
  const response = await shopsApi.setPhotoOrder(
    slug,
    body
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "photos": [
    {
      "id": "LFRvGuTU3D8i334X",
      "url": "/api/blobs/LFRvGuTU3D8i334X"
    },
    {
      "id": "zrySajNyao6_j11x",
      "url": "/api/blobs/zrySajNyao6_j11x"
    }
  ]
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Check Missing Details

Asks Gemini what buyers will want to choose (sizes, colours) and what they'll ask that the
listing doesn't say (dimensions, materials). `createShop` already does this once; call it
again after editing the product. The result is saved as the shop's `suggestions`.

Nothing changes on the shop page until the seller confirms: save options with `updateShop`
(`options`), and answer the questions with `updateShop` (`facts`). Not for recommendations.

```ts
async checkMissingDetails(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<Suggestions>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The suggestions.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`Suggestions`](../../doc/models/suggestions.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await shopsApi.checkMissingDetails(slug);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "oneOfAKind": false,
  "options": [
    {
      "name": "Size",
      "kind": "size",
      "values": [
        {
          "label": "7"
        },
        {
          "label": "8"
        },
        {
          "label": "9"
        }
      ],
      "fromSeller": false
    }
  ],
  "missingFacts": [
    {
      "label": "Wheel size",
      "question": "How big are the wheels?",
      "placeholder": "57 mm"
    }
  ]
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Match Color Photos

Gemini looks at the shop's photos and says which one shows each colour, so buyers who pick a colour see it. Suggests only; save the matches with `setColorPhotos`. Needs at least two photos.

```ts
async matchColorPhotos(
  slug: string,
  body: ApiShopsMatchPhotosRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsMatchPhotosResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `body` | [`ApiShopsMatchPhotosRequest`](../../doc/models/api-shops-match-photos-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: A photo id for each colour Gemini could match. Colours it couldn't place are left out.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsMatchPhotosResponse`](../../doc/models/api-shops-match-photos-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const body: ApiShopsMatchPhotosRequest = {
  colors: [
    {
      label: 'Cream',
      hex: '#f3e9d2',
    },
    {
      label: 'Red',
      hex: '#b3261e',
    }
  ],
};

try {
  const response = await shopsApi.matchColorPhotos(
    slug,
    body
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "matches": {
    "Cream": "LFRvGuTU3D8i334X",
    "Red": "SuqcgNv64HWN8H6e"
  }
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 429 | Over this network's cap for the call (see **Limits**). Retry after `Retry-After` seconds. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Set Color Photos

Saves which photo shows each colour, and nothing else about the shop. Only colours already saved in the shop's options change; `null` clears a colour's photo. A photo id that isn't one of the shop's photos is ignored.

```ts
async setColorPhotos(
  slug: string,
  body: ApiShopsColorPhotosRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsColorPhotosResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `body` | [`ApiShopsColorPhotosRequest`](../../doc/models/api-shops-color-photos-request.md) | Body, Required | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The colours that were changed.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsColorPhotosResponse`](../../doc/models/api-shops-color-photos-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const body: ApiShopsColorPhotosRequest = {
  photos: {
    'Cream': 'LFRvGuTU3D8i334X',
    'Red': 'photos9'
  },
};

try {
  const response = await shopsApi.setColorPhotos(
    slug,
    body
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "saved": [
    "Cream",
    "Red"
  ]
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 400 | Something in the request needs fixing; `error` says what. | [`CustomError`](../../doc/models/custom-error.md) |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


# Connect Pay Pal

Starts PayPal's seller onboarding (Partner Referrals). Send the seller's browser to the
returned `url`; they sign in to PayPal (or create a business account) and grant OneThing
permission. PayPal brings them back to `/api/shops/<slug>/paypal/return`, which saves the
connection and opens the manage page. From then on, sales are paid to that account.

Without a connection, a shop can still go live with a typed `paypal_email`.

**Platforms:** call it with your API key and a `returnUrl`, and PayPal's flow ends on your own
page instead, as `returnUrl?paypal=connected|incomplete|cancelled|failed&shop=<slug>`. The
URL's origin must be one registered on the key.

```ts
async connectPayPal(
  slug: string,
  body?: ApiShopsPaypalRequest,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsPaypalResponse>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `body` | [`ApiShopsPaypalRequest \| undefined`](../../doc/models/api-shops-paypal-request.md) | Body, Optional | - |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Where to send the seller.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsPaypalResponse`](../../doc/models/api-shops-paypal-response.md).

## Example Usage

```ts
const slug = 'raptorrush';

const body: ApiShopsPaypalRequest = {
  returnUrl: 'https://www.miriogames.com/market/manage/?g=7Hq2',
};

try {
  const response = await shopsApi.connectPayPal(
    slug,
    body
  );

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "url": "https://www.sandbox.paypal.com/bizsignup/partner/entry?referralToken=EXAMPLE"
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |
| 503 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Get Pay Pal Connection

Asks PayPal where the seller's connection stands and saves it on the shop. Also finds a connection the seller finished at PayPal without coming back.

```ts
async getPayPalConnection(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiShopsPaypalResponse1>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The connection, or nulls if there is none yet.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiShopsPaypalResponse1`](../../doc/models/api-shops-paypal-response-1.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await shopsApi.getPayPalConnection(slug);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "merchantId": "7MZJ4QXN8K2LA",
  "status": {
    "receivable": true,
    "emailConfirmed": true,
    "checkedAt": "2026-10-21T18:04:11Z"
  }
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |
| 502 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |
| 503 | Gemini, PayPal or Channel3 failed or isn't configured. | [`CustomError`](../../doc/models/custom-error.md) |


# Disconnect Pay Pal

Forgets the connected account. Sales go back to the shop's `paypal_email`.

```ts
async disconnectPayPal(
  slug: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<Ok>>
```

## Authentication

This endpoint requires [AdminToken](../../doc/auth/custom-header-signature.md) **OR** [PlatformKey](../../doc/auth/oauth-2-bearer-token.md)

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `slug` | `string` | Template, Required | The shop's link name (current or former). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: Done.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`Ok`](../../doc/models/ok.md).

## Example Usage

```ts
const slug = 'raptorrush';

try {
  const response = await shopsApi.disconnectPayPal(slug);

  // Extracting fully parsed response body.
  console.log(response.result);

  // Extracting response status code.
  console.log(response.statusCode);
  // Extracting response headers.
  console.log(response.headers);
  // Extracting response body of type `string | Stream`
  console.log(response.body);
} catch (error) {
  if (error instanceof ApiError) {
    // Extracting response error status code.
    console.log(error.statusCode);
    // Extracting response error headers.
    console.log(error.headers);
    // Extracting response error body of type `string | Stream`.
    console.log(error.body);
    if (error instanceof CustomError) {
      console.log(error.result);
    }
  }
}
```

## Example Response *(as JSON)*

```json
{
  "ok": true
}
```

## Errors

| HTTP Status Code | Error Description | Exception Class |
|  --- | --- | --- |
| 404 | No such shop, or the admin token or API key doesn't match it. | [`CustomError`](../../doc/models/custom-error.md) |


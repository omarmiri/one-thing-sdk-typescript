# Links

Short shop links (`/<slug>`).

```ts
const linksApi = new LinksApi(client);
```

## Class Name

`LinksApi`


# Check Link Name

Normalises a proposed link name and says whether a new shop can use it.

:information_source: **Note** This endpoint does not require authentication.

```ts
async checkLinkName(
  name: string,
  requestOptions?: RequestOptions
): Promise<ApiResponse<ApiSlugsCheckResponse>>
```

## Parameters

| Parameter | Type | Tags | Description |
|  --- | --- | --- | --- |
| `name` | `string` | Query, Required | Anything; it is normalised (`Raptor Rush` becomes `raptor-rush`). |
| `requestOptions` | `RequestOptions \| undefined` | Optional | Pass additional request options. |

## Response Type

**200**: The normalised name and whether it can be used.

This method returns an [`ApiResponse`](../../doc/api-response.md) instance. The `result` property of this instance returns the response data which is of type [`ApiSlugsCheckResponse`](../../doc/models/api-slugs-check-response.md).

## Example Usage

```ts
const name = 'Raptor Rush';

try {
  const response = await linksApi.checkLinkName(name);

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
  }
}
```

## Example Response *(as JSON)*

```json
{
  "slug": "raptor-rush",
  "available": true
}
```


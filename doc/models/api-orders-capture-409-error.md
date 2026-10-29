
# Api Orders Capture 409 Error

## Structure

`ApiOrdersCapture409Error`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `status` | [`Status6 \| undefined`](../../doc/models/status-6.md) | Optional | - |

## Example

```ts
try {
  // make the API call
} catch (error) {
  if (error instanceof ApiOrdersCapture409Error) {
    console.log(error.result);
  }
}
```


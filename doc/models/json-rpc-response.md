
# Json Rpc Response

## Structure

`JsonRpcResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `jsonrpc` | `string` | Required, Constant | **Value**: `'2.0'` |
| `id` | [`JsonRpcResponseId \| null`](../../doc/models/containers/json-rpc-response-id.md) | Required | This is a container for one-of cases. |
| `result` | `unknown \| undefined` | Optional | For `tools/call`: `content` (the answer as JSON text), `structuredContent` (the same object) and `isError`. |
| `error` | [`Error1 \| undefined`](../../doc/models/error-1.md) | Optional | - |

## Example

```ts
import { JsonRpcResponse } from 'one-thing-sdk';

const jsonRpcResponse: JsonRpcResponse = {
  jsonrpc: '2.0',
  id: 'String3',
  result: { 'key1': 'val1', 'key2': 'val2' },
  error: {
    code: 24,
    message: 'message4',
  },
};
```


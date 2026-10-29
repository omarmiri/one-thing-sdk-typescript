
# Json Rpc Response

A JSON-RPC 2.0 reply from the MCP server.

## Structure

`JsonRpcResponse`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `jsonrpc` | `string` | Required, Constant | Always `2.0`.<br><br>**Value**: `'2.0'` |
| `id` | [`JsonRpcResponseId \| null`](../../doc/models/containers/json-rpc-response-id.md) | Required | This is a container for one-of cases. |
| `result` | `unknown \| undefined` | Optional | For `tools/call`: `content` (the answer as JSON text), `structuredContent` (the same object) and `isError`. |
| `error` | [`Error \| undefined`](../../doc/models/error.md) | Optional | Present when the call failed. |

## Example

```ts
import { JsonRpcResponse } from 'one-thing-sdk';

const jsonRpcResponse: JsonRpcResponse = {
  jsonrpc: '2.0',
  id: 1,
  result: { 'content': [{ 'type': 'text', 'text': '{ \\'shop\\':  \\'vintageskates\\' }' }], 'isError': false },
};
```



# Json Rpc Request

## Structure

`JsonRpcRequest`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `jsonrpc` | `string` | Required, Constant | **Value**: `'2.0'` |
| `id` | [`JsonRpcRequestId \| null \| undefined`](../../doc/models/containers/json-rpc-request-id.md) | Optional | This is a container for one-of cases. |
| `method` | [`Method`](../../doc/models/method.md) | Required | - |
| `params` | `unknown \| undefined` | Optional | For `tools/call`: `{ name, arguments }`. |

## Example

```ts
import { JsonRpcRequest, Method } from 'one-thing-sdk';

const jsonRpcRequest: JsonRpcRequest = {
  jsonrpc: '2.0',
  method: Method.EnumNotificationsinitialized,
  id: 'String1',
  params: { 'key1': 'val1', 'key2': 'val2' },
};
```


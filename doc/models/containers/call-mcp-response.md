
# Call Mcp Response

## Class Name

`CallMcpResponse`

## Cases

| Type |
|  --- |
| [`JsonRpcResponse`](../../../doc/models/json-rpc-response.md) |
| [`JsonRpcResponse[]`](../../../doc/models/json-rpc-response.md) |

## JsonRpcResponse

### Initialization Code

#### Example

```ts
const value: CallMcpResponse = {
  jsonrpc: '2.0',
  id: 1,
  result: { 'content': [{ 'type': 'text', 'text': '{ \\'shop\\':  \\'vintageskates\\' }' }], 'isError': false },
};
```

## JsonRpcResponse[]

### Initialization Code

#### Example

```ts
const value: CallMcpResponse = [
  {
  jsonrpc: '2.0',
  id: 1,
  result: { 'content': [{ 'type': 'text', 'text': '{ \\'shop\\':  \\'vintageskates\\' }' }], 'isError': false },
}
];
```


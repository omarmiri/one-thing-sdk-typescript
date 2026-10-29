
# Call Mcp Body

## Class Name

`CallMcpBody`

## Cases

| Type |
|  --- |
| [`JsonRpcRequest`](../../../doc/models/json-rpc-request.md) |
| [`JsonRpcRequest[]`](../../../doc/models/json-rpc-request.md) |

## JsonRpcRequest

### Initialization Code

#### Example

```ts
const value: CallMcpBody = {
  jsonrpc: '2.0',
  method: Method.EnumToolscall,
  id: 1,
  params: { 'name': 'get_product', 'arguments': { 'shop': 'vintageskates' } },
};
```

## JsonRpcRequest[]

### Initialization Code

#### Example

```ts
const value: CallMcpBody = [
  {
  jsonrpc: '2.0',
  method: Method.EnumToolscall,
  id: 1,
  params: { 'name': 'get_product', 'arguments': { 'shop': 'vintageskates' } },
}
];
```


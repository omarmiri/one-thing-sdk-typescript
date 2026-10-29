
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
  method: Method.EnumToolslist,
};
```

## JsonRpcRequest[]

### Initialization Code

#### Example

```ts
const value: CallMcpBody = [
  {
  jsonrpc: 'jsonrpc6',
  method: Method.EnumToolslist,
}
];
```


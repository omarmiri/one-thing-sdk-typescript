
# Agent Order Delivery

## Class Name

`AgentOrderDelivery`

## Cases

| Type |
|  --- |
| [`AgentDeliveryAccessCode`](../../../doc/models/agent-delivery-access-code.md) |
| [`AgentDeliveryDownload`](../../../doc/models/agent-delivery-download.md) |
| [`AgentDeliveryShipping`](../../../doc/models/agent-delivery-shipping.md) |

## AgentDeliveryAccessCode

### Initialization Code

#### Example

```ts
const value: AgentOrderDelivery = {
  type: 'access_code',
  accessCode: 'access_code2',
};
```

## AgentDeliveryDownload

### Initialization Code

#### Example

```ts
const value: AgentOrderDelivery = {
  type: 'download',
  downloadUrl: 'download_url8',
};
```

## AgentDeliveryShipping

### Initialization Code

#### Example

```ts
const value: AgentOrderDelivery = {
  type: 'shipping',
  fulfillment: Fulfillment.ToShip,
};
```


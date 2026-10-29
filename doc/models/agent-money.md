
# Agent Money

## Structure

`AgentMoney`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `amount` | `number` | Required | - |
| `currency` | `string` | Required | - |
| `label` | `string \| undefined` | Optional | - |

## Example

```ts
import { AgentMoney } from 'one-thing-sdk';

const agentMoney: AgentMoney = {
  amount: 48.5,
  currency: 'USD',
  label: '$48.50',
};
```


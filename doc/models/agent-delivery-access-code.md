
# Agent Delivery Access Code

A digital product unlocked with an access code.

## Structure

`AgentDeliveryAccessCode`

## Fields

| Name | Type | Tags | Description |
|  --- | --- | --- | --- |
| `type` | `string` | Required, Constant | **Value**: `'access_code'` |
| `accessCode` | `string` | Required | - |
| `startUrl` | `string \| undefined` | Optional | The seller's app with the code filled in. |

## Example

```ts
import { AgentDeliveryAccessCode } from 'one-thing-sdk';

const agentDeliveryAccessCode: AgentDeliveryAccessCode = {
  type: 'access_code',
  accessCode: 'access_code2',
  startUrl: 'start_url4',
};
```



# Custom Header Signature



Documentation for accessing and setting credentials for AdminToken.

## Auth Credentials

| Name | Type | Description | Setter |
|  --- | --- | --- | --- |
| x-admin-token | `string` | The shop's admin token, returned once by `createShop`. Keep it secret. | `xAdminToken` |



**Note:** Auth credentials can be set using `adminTokenCredentials` object in the client.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```ts
import { Client } from 'one-thing-sdk';

const client = new Client({
  adminTokenCredentials: {
    'x-admin-token': 'x-admin-token'
  },
});
```



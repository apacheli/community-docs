# HTTP Endpoints: Interactions

- [Create Interaction Response](#create-interaction-response)

---

## Create Interaction Response

Create a response to an interaction.

This endpoint is specifically for gateway users. Do not use this endpoint if you
are using an outgoing webhook.

```
POST /interactions/{interaction_id}/{interaction_token}/callback
```

### Body Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

### Response

```
Status: 204 No Content
```

```js
// ...
```

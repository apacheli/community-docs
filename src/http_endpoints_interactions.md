# HTTP Endpoints: Interactions

- [Create Interaction Response](#create-interaction-response)

---

## Create Interaction Response

Create a response to an interaction.

This endpoint is specifically for gateway users. Do not use this endpoint if you
are using an outgoing webhook.

```
POST /interactions/:interaction_id/:interaction_token/callback
```

### Path Parameters

| Name                | Type        | Description                        |
| ------------------- | ----------- | ---------------------------------- |
| `interaction_id`    | `Snowflake` | The identifier of the interaction. |
| `interaction_token` | `string`    | The token of the interaction.      |

### Body Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

### Response

```
Status: 204 No Content
```

```json
// ...
```

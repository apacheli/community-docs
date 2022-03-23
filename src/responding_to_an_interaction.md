# Responding to an Interaction

- [Ensuring a Response](#ensuring-a-response)
- [Ephemeral Messages](#ephemeral-messages)
- [Deferring a Response](#deferring-a-response)

---

## Ensuring a Response

Reiterating [the previous section](./using_an_outgoing_webhook.md), all
responses after the verification test must be sent with status `200 OK` and a
`Content-Type` of `application/json`[^1]. The client will see an error message
if you fail to do so.

```https
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "type": 1
}
```

Depending on the type of interaction you receive, you may also have to respond
with the `body` parameter. Providing incorrect values in the response will also
result in an error message that the client will see.

## Ephemeral Messages

If you only want the user that triggered the interaction to see the message, you
can send an ephemeral message. You can set the `flags` of `data` to `0x100` as
defined by the
[`Message Flags` enum table](https://discord.com/developers/docs/resources/channel#message-object-message-flags).

```json
{
  "data": {
    "content": "Hello, World!",
    "flags": 64
  },
  "type": 4
}
```

## Deferring a Response

If you need more time to process the request, you can defer it. **You only have
a 3 second window to respond to Discord.** The `token` of the interaction will
last for 15 minutes before expiring. You can use type `5` as defined by the
[`Interaction Callback Type`](https://discord.com/developers/docs/interactions/receiving-and-responding#interaction-response-object-interaction-callback-type)
enum table.

```json
{
  "type": 5
}
```

---

[^1]: The `application/json` media type specification:
<https://datatracker.ietf.org/doc/html/rfc4627>

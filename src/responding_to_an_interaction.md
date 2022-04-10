# Responding to an Interaction

- [Definitions](#definitions)
  - [Interaction Types](#interaction-types)
  - [Interaction Response](#interaction-response)
  - [Interaction Response Data](#interaction-response-data)
    - [Autocomplete](#autocomplete)
    - [Message](#message)
    - [Modal](#modal)
  - [Interaction Response Data Flags](#interaction-response-data-flags)
  - [Interaction Response Types](#interaction-response-types)
- [Ensuring a Response](#ensuring-a-response)
- [Ephemeral Messages](#ephemeral-messages)
- [Deferring Your Response Message](#deferring-your-response-message)

---

## Definitions

### Interaction Types

These are the types of interactions that can you expect to receive.

| Name                  | Value | Description                                                                                 |
| --------------------- | ----- | ------------------------------------------------------------------------------------------- |
| `PING`                | `1`   | Discord pings for a response. This type is only used for verification testing.              |
| `APPLICATION_COMMAND` | `2`   | The user triggers an application command such as a slash command or a context menu command. |
| `MESSAGE_COMPONENT`   | `3`   | The user triggers a message component such as a button or a select menu.                    |
| `AUTOCOMPLETE`        | `4`   | The client requests for autocomplete results for an application command option.             |
| `MODAL_SUBMIT`        | `5`   | The user submitted a modal.                                                                 |

### Interaction Response

| Name   | Type                                                      | Description |
| ------ | --------------------------------------------------------- | ----------- |
| `data` | [Interaction Response Data](#interaction-response-data)   |             |
| `type` | [Interaction Response Types](#interaction-response-types) |             |

### Interaction Response Data

#### Autocomplete

#### Message

#### Modal

### Interaction Response Types

| Name                      | Value | Description                                                                                               |
| ------------------------- | ----- | --------------------------------------------------------------------------------------------------------- |
| `PONG`                    | `1`   | Respond to `PING` interaction.                                                                            |
| `MESSAGE`                 | `4`   | Respond by sending a message.                                                                             |
| `DEFERRED_MESSAGE`        | `5`   | Defer the message. You are expected to follow up with `MESSAGE` later.                                    |
| `DEFERRED_MESSAGE_UPDATE` | `6`   | Defer the message update. You are expected to follow up with `MESSAGE_UPDATE` later.                      |
| `MESSAGE_UPDATE`          | `7`   | Respond by updating the existing message.                                                                 |
| `AUTOCOMPLETE_RESULT`     | `8`   | Respond with autocomplete results. This type must only be used to respond to `AUTOCOMPLETE` interactions. |
| `MODAL`                   | `9`   | Respond with a modal.                                                                                     |

## Ensuring a Response

Reiterating
[the previous section](./using_an_outgoing_webhook.md#acknowledging-the-ping-interaction),
all responses after the verification test must be sent with status `200 OK` and
a `Content-Type` of `application/json`[^1]. The client will see an error message
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

If you have successfully responded to the interaction, you will gain a `token`
from the interaction. You are allowed to modify the response message for **15
minutes** before the token expires.

## Ephemeral Messages

If you only want the user that triggered the interaction to see the message, you
can send an ephemeral message. You can use the `EPHEMERAL` flag field as defined
by the
[Interaction Response Data Message Flags](#interaction-response-data-message-flags)
table.

```json
{
  "data": {
    "content": "Hello, World!",
    "flags": 64
  },
  "type": 4
}
```

While you are allowed the message even if it is ephemeral, you may not delete
the message unlike a regular message.

## Deferring Your Response Message

You only have a 3 second window to respond to an interaction. If you have not
responded within those 3 seconds, your interaction will automatically be
invalidated. If you need more time to process the interaction, you can defer
your message. To defer your message, You can use type `DEFERRED_MESSAGE` as
defined by the [Interaction Response Types](#interaction-response-types) table.

```json
{
  "type": 5
}
```

Once deferred, the client will see a loading state. This helps the user know
that the bot is taking time to process the request. You are expected to follow
up with a message later.

If you have successfully deferred your message, your time will be increased to
15 minutes. It is important to know that this time increase is not limited to
message deferring&mdash;you are allowed to use interaction HTTP endpoints during
this time.

---

[^1]: The `application/json` media type specification:
<https://datatracker.ietf.org/doc/html/rfc4627>
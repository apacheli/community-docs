# Interactions

- [Overview](#overview)
- [Receiving Interaction Payloads](#receiving-interaction-payloads)
  - [The Flow of Interactions](#the-flow-of-interactions)
  - [Configurable Request URL Method](#configurable-request-url-method)
  - [Gateway Event Method](#gateway-event-method)
- [Verifying Incoming Requests](#verifying-incoming-requests)
- [Acknowledging Incoming Requests](#acknowledging-incoming-requests)

## Overview

_Interactions_ are the new, awesome way to _interact_ with your Discord app.
Lots of cool features such as slash commands, context menu commands, buttons,
select menus, and modals can be found here. This guide will explain the
fundamentals of interactions and how you can integrate them smoothly into your
Discord app.

## The Flow of Interactions

- &#128161; A user interacts with your app (slash command, button, select menu,
  etc).
- &#128229; Your app receives the interaction via request URL or gateway.
- &#128228; Your app responds to the interaction.

Continue reading for more details regarding receiving and responding to
interactions!

## Receiving Interaction Payloads

Whenever a user interacts with your app, Discord will send you a payload
describing the context of that interaction. Your app can receive interaction
payloads via the following methods:

- Configurable request URL
- Gateway event `INTERACTION_CREATE`

It is important to note that these methods of receiving interaction payloads are
_discordant_&mdash;e.g., your app will not receive the payload via gateway event
if a request URL is present.

### Configurable Request URL Method

Your request URL must use a secure protocol such as
[HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/https). You can
configure a URL for app by doing the following:

- Go to your [app's dashboard](https://discord.com/developers/applications)
- Click on _General Information_ on the sidebar
- Add a URL to the _Interactions Endpoint URL_ field

### Gateway Event Method

It is important to note that **the concept of interactions are based on apps as
a whole; they are not exclusive to _bot_ apps**. However, you _must_ turn your
app into a bot app in order to receive interaction payloads if you choose the
gateway event method.

## Verifying Incoming Requests

If you chose the former option, you may have noticed that you are unable to save
the changes that you have made to your app. This is because Discord requires
that you verify incoming requests to prevent unauthorized users from accessing
your URL. Discord uses
[the Ed25519 security signature format](https://ed25519.cr.yp.to/) for
verification. Incoming requests from Discord will use the following format:

```
POST / HTTP/1.1
Content-Type: application/json
X-Signature-Ed25519: XXXXXXXX
X-Signature-Timestamp: XXXXXXXX

(body)
```

You will also need to use your app's public key to verify requests. Following
steps one and two for configuring you app's request URL, right above the input
is where you can copy your app's public key.

Here is an example showing how you can verify requests:

```js
// Your app's public key goes here.
const publicKey = "PUBLIC_KEY";

// The web framework of your choice may differ on how you obtain these fields.
const signature = request.headers.get("X-Signature-Ed25519");
const timestamp = request.headers.get("X-Signature-Timestamp");
const body = await request.text();

// Using the cryptography module of your choice, we will verify the request to
// make sure that the signature is valid.
const verified = await verifyRequest(
  decodeHex(publicKey),
  decodeHex(signature),
  new TextEncoder().encode(timestamp + body),
);

// Respond with `401 Unauthorized` if the signature is invalid.
if (!verified) {
  await respond("401 Unauthorized", { status: 401 });
  return;
}
```

## Acknowledging Incoming Requests

Before you can start handling every other type of interaction, you must first
acknowledge the `Ping` interaction. You will only receive this type of
interaction whenever you try to submit a request URL for your app.

```js
// You must respond with the `application/json` header.
const headers = new Headers();
headers.set("Content-Type", "application/json; charset=utf8");

// Discord will send you the context of the interaction in the JSON format.
const body = await response.json();

// If the type of the interaction is `1`, Discord is pinging us, so we reply
// reply back with `Pong`.
if (body.type === 1) {
  // Effectively a `Pong` response.
  const data = {
    type: 1,
  };

  // Respond with `200 OK`.
  await respond(JSON.stringify(data), {
    headers: headers,
    status: 200,
  });
  return;
}
```

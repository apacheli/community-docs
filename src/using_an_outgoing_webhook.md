# Using an Outgoing Webhook

To configure your application's outgoing webhook:

- Go to your
  [application's dashboard](https://discord.com/developers/applications).
- Click on _General Information_ tab on the sidebar.
- Set your URL in the _Interactions Endpoint URL_ field.

Your interactions endpoint URL must use
[HTTPS](https://en.wikipedia.org/wiki/HTTPS). A verification test begins when
you submit your URL. Discord will send you two requests&mdash;one with and one
_without_ a valid signature. You must be ready to verify these requests ahead of
time before your URL can be saved.

Discord uses [the Ed25519 security signature format](https://ed25519.cr.yp.to/)
for verification. To prevent unauthorized access to your URL, Discord sends you
two signature headers:

- `X-Signature-Ed25519`
- `X-Signature-Timestamp`

Incoming requests from Discord use the following format:

```https
POST / HTTP/1.1
Content-Type: application/json
X-Signature-Ed25519: XXXXXXXX
X-Signature-Timestamp: XXXXXXXX
```

```json
{
  "application_id": "XXXXXXXX",
  "id": "XXXXXXXX",
  "token": "XXXXXXXX",
  "type": 1,
  "user": {
    "avatar": null,
    "discriminator": "0000",
    "id": "XXXXXXXX",
    "public_flags": 0,
    "username": "XXXXXXXX"
  },
  "version": 1
}
```

Next, you will need your application's public key. You can obtain it by going to
your [application's dashboard](https://discord.com/developers/applications).

Here is an example using [express](https://github.com/expressjs/express) and
[tweetnacl](https://github.com/dchest/tweetnacl-js) that shows you how to verify
requests:

```js
import express from "express";
import nacl from "tweetnacl";

const publicKey = "PUBLIC_KEY";

const body = req.body; // Raw UTF-8 text
const signature = req.header("X-Signature-Ed25519");
const timestamp = req.header("X-Signature-Timestamp");

const verified = nacl.sign.detached.verify(
  Buffer.from(timestamp + body),
  Buffer.from(signature, "hex"),
  Buffer.from(publicKey, "hex"),
);
```

If the verification fails, respond with `401 Unauthorized`.

```js
if (!verified) {
  res.status(401).send("401 Unauthorized");
  return;
}
```

The final part of the test is acknowledged the ping interaction. You will only
receive this type of interaction when you submit a new interactions endpoint
URL. You must respond with status `200 OK` and a `Content-Type` of
`application/json`.

```js
if (interaction.type === 1) {
  res.status(200).send({ type: 1 });
  return;
}
```

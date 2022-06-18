# The Flow of Interactions

- [Overview](#overview)
- [Interruptions in Interaction Flows](#interruptions-in-interaction-flows)

---

## Overview

Let us take a look at how interactions work.

First, A user creates an interaction by triggering one of your application's
[entry points](./glossary.md#entry-point). For example, the following are entry
points:

- A user invoking a slash command
- A user clicking on a button
- A user selecting an option on a select menu

Next, your application will receive the interaction payload. Your application
has two ways of receiving them:

- Outgoing webhook
- Gateway event `INTERACTION_CREATE`

It is important to note that these methods are mutually exclusive&mdash;your
application will not receive the interaction payload via gateway event if an
outgoing webhook is present.

Knowing the context of the interaction, your application can _and must_ properly
respond to it.

## Interruptions in Interaction Flows

A typical interaction flow can be interrupted when:

- The client asks for autocomplete results for an application command option.
- Your application sends a modal to the user.

These interaction types must be resolved differently than a typical interaction
flow. While most interactions must conclude with a message, there are
interaction types that may find their way in the middle of the flow.
